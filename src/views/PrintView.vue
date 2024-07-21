<template>
  <main ref="main">
    <section class="no-print">
      <PrintFormatToolbar
        v-model:columns="columns"
        v-model:fontSize="fontSize"
      />
    </section>
    <div
      class="print-view"
      :style="{
        '--font-size': `${fontSize}px`,
        '--transform-scale': transformScale,
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

const A4_WIDTH = 595; // Width of A4 paper in points
const columns = ref(1);
const fontSize = ref(16);
const main = ref(null);
const mainWidth = ref(A4_WIDTH);
const transformScale = computed(computeTransformScale);

onMounted(() => {
  updateMainWidth();
  window.addEventListener('resize', updateMainWidth);
});

onUnmounted(() => {
  window.removeEventListener('resize', updateMainWidth);
});

function computeTransformScale() {
  const widthInPt = mainWidth.value * 3 / 4;
  return widthInPt < A4_WIDTH ? widthInPt / A4_WIDTH : 1;
}

function updateMainWidth() {
  mainWidth.value = main.value.clientWidth;
}
</script>

<style scoped>
.print-view {
  font-size: var(--font-size, 16pt);
  height: 894pt;
  margin: 0 auto;
  outline: 1px solid #ccc;
  padding: 2em;
  transform: scale(var(--transform-scale));
  transform-origin: top left;
  width: 595pt;

  @media print {
    outline: none;
    padding: 0;
    transform: none;
  }
}
</style>

