<script setup>
import { defineAsyncComponent, ref, watch } from 'vue';
const input = ref(localStorage.getItem('chordproInput') || '');
const ChordChart = defineAsyncComponent(() => import('../components/ChordChart.vue'));

watch(input, (value) => {
  localStorage.setItem('chordproInput', value);
});
</script>

<template>
  <main>
    <div class="chordpro-textfield">
      <label for="chordproInput">Input the ChordPro chart here:</label>
      <textarea
        v-model="input"
        id="chordproInput"
        rows="10"
      />
    </div>
    <section>
      <ChordChart
        :chord-pro="input"
      />
    </section>
  </main>
</template>

<style scoped>
.chordpro-textfield {
  flex-direction: column;
  gap: 8px;
  display: flex;
}
</style>
