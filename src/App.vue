<template>
  <header class="no-print">
    <nav>
      <RouterLink to="/">Home</RouterLink>
      <RouterLink to="/print">Print Layout</RouterLink>
    </nav>
  </header>
  <RouterView />
</template>

<script setup>
import { provide, ref } from 'vue';
import { RouterLink, RouterView } from 'vue-router'

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
nav {
  display: flex;
  justify-content: center;
  gap: 1rem;
  padding: 1rem;
  background-color: #f0f0f0;
}
</style>
