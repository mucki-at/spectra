<template>
  <div id="app">
    <spectrum
      :low = 370
      :high = 795
      :byFreq = false
      >
    </spectrum>
    <spectrum
      :low = 800
      :high = 370
      :byFreq = true
      >
    </spectrum>
  </div>
</template>

<script setup lang="ts">
import Spectrum from './components/Spectrum.vue';
</script>

<style>
html,
body {
  margin: 0;
  padding: 0;
}

#app {
  height: 100vh;
  width: 100vw;
  box-sizing: border-box;
}

</style>