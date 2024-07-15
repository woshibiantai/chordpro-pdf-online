<template>
  <section>
    <h1>{{ title }}</h1>
    <p>{{ artist }}</p>
    <p>
      <span>{{ key }}</span>
      <span>{{ time }}</span>
      <span>{{ tempo }}</span>
    </p>
    <ChordChartBodyLine
      v-for="(line, index) in bodyLines"
      :key="index"
      :line="line"
    />
  </section>
</template>

<script setup>
import { computed } from 'vue';
import ChordChartBodyLine from './ChordChartBodyLine.vue';

const props = defineProps({
  chordPro: {
    type: String,
    default: '',
  },
});

const lines = computed(() => props.chordPro.split('\n'));

const artist = computed(() => {
  const artistLine = lines.value.find(line => line.startsWith('{artist:')) || '';
  return artistLine.replace('{artist:', '').replace('}', '');
});
const bodyLines = computed(() => lines.value.filter(line => !line.startsWith('{') || line.startsWith('{comment:')));
const key = computed(() => {
  const keyLine = lines.value.find(line => line.startsWith('{key:')) || '';
  return keyLine.replace('{key:', '').replace('}', '');
});
const tempo = computed(() => {
  const tempoLine = lines.value.find(line => line.startsWith('{tempo:')) || '';
  return tempoLine.replace('{tempo:', '').replace('}', '');
});
const time = computed(() => {
  const timeLine = lines.value.find(line => line.startsWith('{time:')) || '';
  return timeLine.replace('{time:', '').replace('}', '');
});
const title = computed(() => {
  const titleLine = lines.value.find(line => line.startsWith('{title:')) || '';
  return titleLine.replace('{title:', '').replace('}', '');
});
</script>
