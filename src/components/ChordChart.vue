<template>
  <section class="chordchart">
    <h1>{{ title }}</h1>
    <p class="chordchart-artist">{{ artist }}</p>
    <p class="chordchart-metadata-line">
      <span
        v-if="key"
        class="chordchart-metadata"
      >
        Key - {{ key }}
      </span>
      <span
        v-if="tempo"
        class="chordchart-metadata"
      >
        Tempo - {{ tempo }}
      </span>
      <span
        v-if="time"
        class="chordchart-metadata"
      >
        Time - {{ time }}
      </span>
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

<style scoped>
.chordchart {
  aspect-ratio: 1 / 1.414;
  outline: 1px solid #ccc;
  padding: 2em;

  @media print {
    outline: none;
  }
}

h1 {
  font-size: 1.5em;
  font-weight: bold;
  line-height: 1.2;
}

.chordchart-artist {
  font-size: .7em;
}

.chordchart-metadata-line {
  display: flex;
  gap: .4em;

}

.chordchart-metadata {
  font-size: 0.8em;
  font-weight: bold;
  
  &:not(:last-child) {
    &::after {
      content: '|';
      font-weight: bold;
      margin-left: .4em;
    }
  }
}
</style>
