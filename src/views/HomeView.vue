<template>
  <main>
    <section class="no-print">
      <ChordProInput
        v-model:input="input"
      />
      <Toolbar
        v-model:columns="columns"
      />
    </section>
    <ChordChart
      :columns="columns"
      :chord-pro="input"
    />
  </main>
</template>

<script setup>
import { defineAsyncComponent, ref, watch } from 'vue';
const input = ref(localStorage.getItem('chordproInput') || '');
const ChordProInput = defineAsyncComponent(() => import('../components/ChordProInput.vue'));
const ChordChart = defineAsyncComponent(() => import('../components/ChordChart.vue'));
const Toolbar = defineAsyncComponent(() => import('../components/Toolbar.vue'));

const columns = ref(1);

import('@/assets/sample-chart.js')
  .then(({ sampleChordProChart }) => {
    if (!input.value) {
      input.value = sampleChordProChart;
    }
  });

watch(input, (value) => {
  localStorage.setItem('chordproInput', value);
});
</script>


<style scoped>
.chordpro-print-button {
  margin-bottom: 8px;
  margin-top: 8px;
}
</style>
