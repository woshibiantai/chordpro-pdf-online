<template>
  <main>
    <form class="no-print">
      <label for="chordpro-input">Chord chart in ChordPro format:</label>
      <textarea
        ref="textareaRef"
        v-model="input"
        id="chordpro-input"
        rows="10"
      />
    </form>
    <ChordChart
      @click="onChordChartClick"
    />
  </main>
</template>

<script setup>
import { inject, defineAsyncComponent, ref } from 'vue';
const ChordChart = defineAsyncComponent(() => import('../components/ChordChart.vue'));
const input = inject('chordProInput');
const textareaRef = ref(null);

function onChordChartClick(event) {
  textareaRef.value.focus();
  const closestElementWithIndex = event.target.closest('[data-index]');
  if (closestElementWithIndex) {
    const index = parseInt(closestElementWithIndex.getAttribute('data-index'), 10);
    textareaRef.value.setSelectionRange(index, index);
  }
};
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

label {
  display: block;
  font-size: 12px;
  font-weight: bold;
  margin-bottom: 4px;
}

textarea {
  font-family: monospace;
  font-size: 14px;
  width: 100%;
}
</style>
