<template>
  <div id="app">
      <Countdown date="January 1, 2018"></Countdown>
  </div>
</template>

<script>
import Countdown from './components/Countdown.vue';

export default {
    components: { Countdown },
}
</script>

<style>
#app {
    background-color: #34495e;
    bottom: 0;
    left: 0;
    position: absolute;
    right: 0;
    top:0;
}
</style>
