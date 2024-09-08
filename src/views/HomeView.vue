<template>
  <main>
    <form
      class="no-print"
      @submit.prevent
    >
      <label for="chordpro-input">Chord chart in {{ format }} format:</label>
      <textarea
        ref="textareaRef"
        v-model="input"
        id="chordpro-input"
        rows="10"
      />

      <fieldset>
        <legend>Settings</legend>
        <label for="chordpro-transpose">Transpose: </label>
        <input
          type="number"
          id="chordpro-transpose"
          v-model="transposition"
          max="12"
          min="-12"
          required
        >

        <label for="chordpro-format">Format:</label>
        <select
          id="chordpro-format"
          v-model="format"
          required
        >
          <option value="chordpro" default>ChordPro</option>
          <option value="ultimate-guitar">Ultimate Guitar</option>
        </select>
      </fieldset>
    </form>
    <ChordChart
      :transposition="validatedTransposition"
      @click="onChordChartClick"
    />
  </main>
</template>

<script setup>
import { computed, inject, defineAsyncComponent, provide, ref, watch } from 'vue';
const ChordChart = defineAsyncComponent(() => import('../components/ChordChart.vue'));
const input = inject('chordProInput');
const format = inject('format');
const textareaRef = ref(null);
const caretPosition = ref(0);
const transposition = ref(0);
provide('caretPosition', caretPosition);

watch(input, () => {
  caretPosition.value = getCaretPosition();
});

const validatedTransposition = computed(() => {
  const transpositionWithDefault = transposition.value || 0;
  return Math.min(12, Math.max(-12, transpositionWithDefault || 0));
});

function onChordChartClick(event) {
  const closestElementWithIndex = event.target.closest('[data-index]');
  if (closestElementWithIndex) {
    const index = parseInt(closestElementWithIndex.getAttribute('data-index'), 10);
    caretPosition.value = index;
    textareaRef.value.setSelectionRange(caretPosition.value, caretPosition.value);
  }
  textareaRef.value.focus();
};

function getCaretPosition() {
  return textareaRef.value.selectionStart;
}
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

input:invalid {
  border-color: red;
}

fieldset {
  display: flex;
  gap: 8px;
}
</style>
