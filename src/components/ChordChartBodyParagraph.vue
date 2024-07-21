<template>
  <div class="chordchart-body-paragraph">
    <ChordChartLine
      v-for="(line, index) in props.paragraph.lines"
      :key="index"
      :line="line"
    />
  </div>
</template>

<script setup>
import { Paragraph } from 'chordsheetjs';
import ChordChartLine from './ChordChartLine.vue';

const props = defineProps({
  paragraph: {
    type: Paragraph,
    required: true,
  }
});
</script>

<style scoped>
.chordchart-body-paragraph {
  break-inside: avoid;
  padding-top: 1em;
}
</style>
