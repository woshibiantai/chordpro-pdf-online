<template>
  <main>
    <ChordChart />
  </main>
</template>

<script setup>
import { defineAsyncComponent } from 'vue';
const ChordChart = defineAsyncComponent(() => import('../components/ChordChart.vue'));
</script>
