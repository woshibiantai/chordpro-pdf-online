<template>
  <span
    v-if="isTag"
    class="chordchart-body-tag"
  >
    {{ item.value }}
  </span>
  <ChordChartChordLyricsPair
    v-else
    :pair="item"
  />
</template>

<script setup>
import { ChordLyricsPair, Tag } from 'chordsheetjs';
import ChordChartChordLyricsPair from './ChordChartChordLyricsPair.vue';
const props = defineProps({
  item: {
    type: [ChordLyricsPair, Tag],
    required: true,
  }
});
const isTag = props.item instanceof Tag && props.item.hasValue();
</script>

<style scoped>
.chordchart-body-tag {
  font-size: 1em;
  font-weight: bold;
  line-height: 2;
}
</style>
