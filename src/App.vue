<template>
  <div id="app">
    <FeeCalculator />
  </div>
</template>

<script>
import FeeCalculator from './components/FeeCalculator.vue';

export default {
  name: 'App',
  components: {
    FeeCalculator
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  margin-top: 20px;
}
</style>
