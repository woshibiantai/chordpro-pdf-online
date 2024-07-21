<template>
  <p
    class="chordchart-body-paragraph-line"
    :class="{
      'chordchart-body-paragraph-line-chorus': line.type === 'chorus',
    }"
    :data-index="indexOfEndOfLine"
  >
    <ChordChartLineItem
      v-for="(item, index) in line.items"
      :key="index"
      :item="item"
      :data-index="item.endIndex"
    />
  </p>
</template>

<script setup>
import { computed, inject } from 'vue';
import { Line } from 'chordsheetjs';
import ChordChartLineItem from './ChordChartLineItem.vue';

const props = defineProps({
  line: {
    type: Line,
    required: true,
  }
});

const chordProInput = inject('chordProInput');
const lineBreakMatches = computed(() => [...chordProInput.value.matchAll(/\n/g)]);
const indexOfEndOfLine = computed(() => lineBreakMatches.value[props.line.lineNumber].index);
</script>

<style scoped>
.chordchart-body-paragraph-line {
  display: flex;
  flex-wrap: wrap;
}

.chordchart-body-paragraph-line-chorus {
  border-left: 1px solid #000;
  
  &:last-child {
    padding-bottom: 1.1em;
  }
}
</style>
