<template>
  <div id="app">
    <h1>Meter - Viotas Assessment</h1>
    <Main />
    <MultiplesGraphs />
  </div>
</template>

<script>
import Main from './components/Main.vue'
import MultiplesGraphs from './components/MultiplesGraphs.vue'

export default {
  name: 'App',
  components: {
    Main,
    MultiplesGraphs
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-top: 0px;
  background-color: gray;
}

</style>
