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

function onPrint() {
  window.print();
}
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
      <button
        class="chordpro-print-button"
        @click="onPrint()"
        type="button"
      >
        Print
      </button>
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

.chordpro-print-button {
  margin-bottom: 8px;
  margin-top: 8px;
}
</style>
