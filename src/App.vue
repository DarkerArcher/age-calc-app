<template>
  <AgeCalc />
</template>

<script>
import AgeCalc from './components/AgeCalc.vue'

export default {
  name: 'App',
  components: {
    AgeCalc
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
