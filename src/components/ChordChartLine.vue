<template>
  <p class="chordchart-body-paragraph-line">
    <ChordChartLineItem
      v-for="(item, index) in line.items"
      :key="index"
      :item="item"
    />
  </p>
</template>

<script setup>
import { Line } from 'chordsheetjs';
import ChordChartLineItem from './ChordChartLineItem.vue';

defineProps({
  line: {
    type: Line,
    required: true,
  }
});
</script>

<style scoped>
.chordchart-body-paragraph-line {
  display: flex;
  flex-wrap: wrap;
}

.chordchart-body-paragraph-line-verse {
  margin-left: 2em;
}
</style>
