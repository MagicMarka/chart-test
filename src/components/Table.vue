<template>
  <div class="table-wrapper">
      <table>
         <thead>
          <tr>
            <th>Дата</th>
            <th v-if="traffic[0]" @click="drawChart(traffic, 'Трафик')">Трафик</th>
            <th v-if="send[0]" @click="drawChart(send, 'Отправка')">Отправка</th>
            <th v-if="deliver[0]" @click="drawChart(deliver, 'Доставка')">Доставка</th>
            <th v-if="opens[0]" @click="drawChart(opens, 'Открытие')">Открытие</th>
            <th v-if="clicks[0]" @click="drawChart(clicks, 'Переходы')">Переходы</th>
            <th v-if="answers[0]" @click="drawChart(answers, 'Ответы')">Ответы</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(entry, index) in tableData" :key="index">
            <td>{{entry.date}}</td>
            <td v-if="traffic[0]"><input v-model="entry['metrics']['traffic']" v-on:keyup.self="drawChart(traffic, 'Трафик')"></td>
            <td v-if="send[0]"><input v-model="entry['metrics']['send']" v-on:keyup="drawChart(send, 'Отправка')"></td>
            <td v-if="deliver[0]"><input v-model="entry['metrics']['deliver']" v-on:keyup="drawChart(deliver, 'Доставка')"></td>
            <td v-if="opens[0]"><input v-model="entry['metrics']['opens']" v-on:keyup="drawChart(opens, 'Открытие')"></td>
            <td v-if="clicks[0]"><input v-model="entry['metrics']['clicks']" v-on:keyup="drawChart(clicks, 'Переходы')"></td>
            <td v-if="answers[0]"><input v-model="entry['metrics']['answers']" v-on:keyup="drawChart(answers, 'Ответы')"></td>
          </tr>
          <tr>
            <th>Разница %</th>
            <td v-if="traffic[0]">{{findLargestDifference('traffic')}}</td>
            <td v-if="send[0]">{{findLargestDifference('send')}}</td>
            <td v-if="deliver[0]">{{findLargestDifference('deliver')}}</td>
            <td v-if="opens[0]">{{findLargestDifference('opens')}}</td>
            <td v-if="clicks[0]">{{findLargestDifference('clicks')}}</td>
            <td v-if="answers[0]">{{findLargestDifference('answers')}}</td>
          </tr>
          <tr>
            <td></td>
            <td v-if="traffic[0]" @click="deleteCol('traffic')">Delete</td>
            <td v-if="send[0]" @click="deleteCol('send')">Delete</td>
            <td v-if="deliver[0]" @click="deleteCol('deliver')">Delete</td>
            <td v-if="opens[0]" @click="deleteCol('opens')">Delete</td>
            <td v-if="clicks[0]" @click="deleteCol('clicks')">Delete</td>
            <td v-if="answers[0]" @click="deleteCol('answers')">Delete</td>
          </tr>
        </tbody>
      </table>
  </div>
</template>

<script>
  import { EventBus } from '../main.js';
  const axios = require('axios');
  export default {
    name: 'Table',
    data() {
      return {
        tableData: [],
        dates: [],
        metrics: ['traffic', 'send', 'deliver', 'opens', 'clicks', 'answers'],
      };
    },
    methods: {
      getData() {
        axios.get('https://magicmarka.github.io/table/data.json')
                .then((response) => {
                  this.tableData = response.data;
                  this.dates = response.data.map(entry => entry.date);

                })
      },

      deleteCol(name) {
        this.tableData.map(entry => {entry.metrics[name] = ''});
        EventBus.$emit('hide-chart');
      },
      drawChart(metric, name) {
        EventBus.$emit('draw-chart', this.dates, metric, name);
      },
      findLargestDifference(arrayName) {
         let array = this[arrayName];
         if (array.length <= 1) return -1;

         let currentMin = array[0];
         let currentMaxDifference = 0;

         for (var i = 1; i < array.length; i++) {
          if (array[i] > currentMin && (array[i] - currentMin > currentMaxDifference)) {
            currentMaxDifference = array[i] - currentMin;
            }
          else  if (array[i] <= currentMin) {
              currentMin = array[i];
            }
          }
         return currentMaxDifference / 100  ;
        },

    },
    computed: {
        traffic() {
          return this.tableData.map(entry => entry.metrics.traffic)
        },
        send() {
          return this.tableData.map(entry => entry.metrics.send)
        },
        deliver() {
          return this.tableData.map(entry => entry.metrics.deliver)
        },
        opens() {
          return this.tableData.map(entry => entry.metrics.opens);
        },
        clicks() {
          return this.tableData.map(entry => entry.metrics.clicks);
        },
        answers() {
          return this.tableData.map(entry => entry.metrics.answers);
        },

      },
    mounted() {
      this.getData();
    },


  }


</script>


<style scoped>

</style>
