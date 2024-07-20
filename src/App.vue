<template>
  <header class="no-print">
    <nav>
      <RouterLink to="/">Home</RouterLink>
    </nav>
  </header>
  <ChordProInput
    v-model:input="chordProInput"
  />
  <RouterView />
</template>

<script setup>
import { defineAsyncComponent, provide, ref } from 'vue';
import { RouterLink, RouterView } from 'vue-router'

const ChordProInput = defineAsyncComponent(() => import('./components/ChordProInput.vue'));

const chordProInput = ref('');

provide('chordProInput', chordProInput);

import('@/assets/sample-chart.js')
  .then(({ sampleChordProChart }) => {
    if (!chordProInput.value) {
      chordProInput.value = sampleChordProChart;
    }
  });
</script>

<style scoped>
</style>
