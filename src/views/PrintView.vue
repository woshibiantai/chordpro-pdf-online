<template>
  <main ref="main">
    <section class="no-print">
      <PrintFormatToolbar
        v-model:columns="columns"
      />
    </section>
    <div
      class="print-view"
      :style="{
        '--transform-scale': transformScale,
        '--width': `${MAIN_MAX_WIDTH}px`
      }"
    >
      <ChordChart
        :columns="columns"
      />
    </div>
  </main>
</template>

<script setup>
import { computed, defineAsyncComponent, onMounted, onUnmounted, ref } from 'vue';
const ChordChart = defineAsyncComponent(() => import('../components/ChordChart.vue'));
const PrintFormatToolbar = defineAsyncComponent(() => import('../components/PrintFormatToolbar.vue'));

const MAIN_MAX_WIDTH = 1216;
const main = ref(null);
const columns = ref(1);
const mainWidth = ref(MAIN_MAX_WIDTH);
const transformScale = computed(computeTransformScale);

onMounted(() => {
  updateMainWidth();
  window.addEventListener('resize', updateMainWidth);
});

onUnmounted(() => {
  window.removeEventListener('resize', updateMainWidth);
});

function computeTransformScale() {
  return mainWidth.value < MAIN_MAX_WIDTH ? mainWidth.value / MAIN_MAX_WIDTH : 1;
}

function updateMainWidth() {
  mainWidth.value = main.value.clientWidth;
}
</script>

<style scoped>
.print-view {
  aspect-ratio: 1 / 1.414;
  outline: 1px solid #ccc;
  padding: 2em;
  transform: scale(var(--transform-scale));
  transform-origin: top left;
  width: var(--width);

  @media print {
    transform: none;
    outline: none;
  }
}
</style>

