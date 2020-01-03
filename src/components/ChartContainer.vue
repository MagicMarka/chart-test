<template>
  <div class="container">
    <line-chart class="chart" :height="500"
      v-if="loaded"
      :chartData="chartData"
      :options="options"
      />
  </div>
</template>

<script>
import LineChart from './Chart.vue'
import { EventBus } from '../main.js';



export default {
  name: 'LineChartContainer',
  components: { LineChart },
  data() {
      return {
          loaded: false,
          chartData: null,
          options: {
              maintainAspectRatio:false,
              responsive: true,
              tooltips: {
                mode: 'point'
            },
          }
      };
  },
  methods: {
      fillData(dates, content, name) {
        this.chartData = {
          labels: dates,
          datasets: [ {
              borderColor:'rgba(255, 99, 132, 1)',
              borderWidth: 1,
              label: name,
              data: content
            }
          ],

        }
      },
  },
  mounted() {
      EventBus.$on('draw-chart', (dates, content, name) => {
          this.loaded = true;
          this.fillData(dates, content, name);
    });
  EventBus.$on('hide-chart',() => {this.loaded = false});
  },


}
</script>
<style>


</style>
