<template>
  <main>
    <ChordChart />
  </main>
</template>

<script setup>
import { defineAsyncComponent } from 'vue';
const ChordChart = defineAsyncComponent(() => import('../components/ChordChart.vue'));
</script>

<style scoped>
main {
  font-size: 16px;

  @media screen and (max-width: 1280px) {
    font-size: 14px;
  }

  @media screen and (max-width: 1024px) {
    font-size: 13px;
  }

  @media screen and (max-width: 875px) {
    font-size: 12px;
  }

  @media screen and (max-width: 768px) {
    font-size: 11px;
  }

  @media screen and (max-width: 576px) {
    font-size: 10px;
  }

  @media screen and (max-width: 475px) {
    font-size: 9px;
  }
}
</style>
