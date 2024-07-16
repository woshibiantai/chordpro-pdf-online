<script setup>
import { defineAsyncComponent, ref, watch } from 'vue';
const input = ref(localStorage.getItem('chordproInput') || '');
const ChordChart = defineAsyncComponent(() => import('../components/ChordChart.vue'));

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

<template>
  <main>
    <section class="no-print">
      <div class="chordpro-textfield">
        <label for="chordproInput">Input the ChordPro chart here:</label>
        <textarea
          v-model="input"
          id="chordproInput"
          rows="10"
        />
      </div>
    </section>
    <ChordChart
      :chord-pro="input"
    />
  </main>
</template>

<style scoped>
.chordpro-textfield {
  flex-direction: column;
  gap: 8px;
  display: flex;
}
</style>
