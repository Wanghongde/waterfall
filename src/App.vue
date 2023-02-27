<template>
  <div class="container">
    <water-fall></water-fall>
  </div>
</template>

<script setup lang='ts'>
import WaterFall from './components/water-fall.vue'

</script>

<style scoped>
:global(html) {
  width: 100%;
  height: 100%;
}
:global(body) {
  width: 100%;
  height: 100%;
  margin: 0;
}
:global(#app) {
  width: 100%;
  height: 100%;
}

.container {
  width: 100%;
  height: 100%;
}
</style>