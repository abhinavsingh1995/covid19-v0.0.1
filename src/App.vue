<template>
  <div id="app" class="container">
    <div class="row mt-5">
      <div class="col">
        <h2>Positive</h2>
        <line-chart v-if="arrPositive.length > 0" :chartData="arrPositive" :options="chartOptions" label="Positive"></line-chart>
      </div>
    </div>
    
  </div>
</template>

<script>
import axios from "axios";
import moment from 'moment';
import LineChart from './components/LineChart.vue';

export default {
  name: 'App',
  components: {
    LineChart
  },
  data() {
    return {
      arrPositive: [],
      arrHospitalized: [],
      arrInIcu: [],
      arrOnVentilators: [],
      arrRecovered: [],
      arrDeaths: [],
      chartOptions: 
      {
        responsive: true,
        maintainAspectRatio: false
      }
    }
  },

  async created() {
  const {data} = await axios.get("https://covidtracking.com/api/us/daily");
    // console.log(data);

    data.forEach(d => {
      const date = moment(d.date, 'YYYYMMDD').format('DDMM');
      const {
        death,
        hospitalizedCurrently,
        inIcuCurrently,
        onVentilatorCurrently,
        positive,
        recovered
      } = d;


      this.arrDeaths.push({date, total: death});
      this.arrPositive.push({date, total: positive});
      this.arrHospitalized.push({date, total: hospitalizedCurrently});
      this.arrInIcu.push({date, total: inIcuCurrently});
      this.arrOnVentilators.push({date, total: onVentilatorCurrently});
      this.arrRecovered.push({date, total: recovered});

      // console.log(this.$data);

    });
  },
}
</script>

<style>

</style>
