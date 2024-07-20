<template>
  <main>
    <section class="no-print">
      <Toolbar
        v-model:columns="columns"
      />
    </section>
    <ChordChart
      :columns="columns"
    />
  </main>
</template>

<script setup>
import { defineAsyncComponent, ref } from 'vue';
const ChordChart = defineAsyncComponent(() => import('../components/ChordChart.vue'));
const Toolbar = defineAsyncComponent(() => import('../components/Toolbar.vue'));

const columns = ref(1);
</script>


<style scoped>
.chordpro-print-button {
  margin-bottom: 8px;
  margin-top: 8px;
}
</style>
