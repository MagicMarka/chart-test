<template>
  <div id="app">
    <ChartContainer/>
    <Table/>
  </div>
</template>

<script>
import Table from './components/Table.vue'
import ChartContainer from './components/ChartContainer.vue'

export default {
  name: 'app',
  components: {
    Table,
    ChartContainer
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
