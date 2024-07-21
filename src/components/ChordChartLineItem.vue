<template>
  <ChordChartTag
    v-if="isTag"
    :tag="item"
  />
  <ChordChartChordLyricsPair
    v-else-if="isChordLyricsPair"
    :pair="item"
  />
</template>

<script setup>
import { ChordLyricsPair, Comment, Tag } from 'chordsheetjs';
import ChordChartChordLyricsPair from './ChordChartChordLyricsPair.vue';
import ChordChartTag from './ChordChartTag.vue';

const props = defineProps({
  item: {
    type: [ChordLyricsPair, Tag, Comment],
    required: true,
  }
});

const isChordLyricsPair = props.item instanceof ChordLyricsPair;
const isTag = props.item instanceof Tag && props.item.hasValue();
</script>
