<template>
  <div id="app">
      <Countdown></Countdown>
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
    background-color: #000;
    bottom: 0;
    left: 0;
    position: absolute;
    right: 0;
    top:0;
}
</style>
