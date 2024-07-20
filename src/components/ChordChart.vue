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
    <div
      class="chordchart-body"
      :style="{ '--chordchart-columns': columns }"
    >
      <div
        class="chordchart-body-stanza"
        v-for="(stanza, index) in stanzas"
        :key="index"
      >
        <ChordChartBodyLine
          v-for="(line, index) in stanza"
          :key="index"
          :line="line"
        />
      </div>
    </div>
  </section>
</template>

<script setup>
import { computed, inject } from 'vue';
import ChordChartBodyLine from './ChordChartBodyLine.vue';

defineProps({
  columns: {
    type: Number,
    default: 1,
  },
});

const chordProInput = inject('chordProInput');
const lines = computed(() => chordProInput.value.split('\n'));
const artist = computed(computeArtist);
const bodyLines = computed(computeBodyLines);
const key = computed(computeKey);
const stanzas = computed(computeStanzas);
const tempo = computed(computeTempo);
const time = computed(computeTime);
const title = computed(computeTitle);

function computeArtist() {
  const artistLine = lines.value.find(line => line.startsWith('{artist:')) || '';
  return artistLine.replace('{artist:', '').replace('}', '');
}

function computeBodyLines() {
  return lines.value.filter(line => (!line.startsWith('{') || line.startsWith('{comment:')));
}

function computeKey() {
  const keyLine = lines.value.find(line => line.startsWith('{key:')) || '';
  return keyLine.replace('{key:', '').replace('}', '');
}

function computeStanzas() {
  return bodyLines.value.reduce((acc, line, index) => {
    if (index === 0) {
      acc.push(line.trim() === '' ? [] : [line]); // Start first stanza
    } else if (line.trim() === '') {
      if (acc[acc.length - 1].length === 0) {
        return acc; // Skip if previous stanza was empty
      }
      acc.push([]); // Start new stanza
    } else {
      acc[acc.length - 1].push(line); // Add line to current stanza
    }
    return acc;
  }, []);
}

function computeTempo() {
  const tempoLine = lines.value.find(line => line.startsWith('{tempo:')) || '';
  return tempoLine.replace('{tempo:', '').replace('}', '');
}

function computeTime() {
  const timeLine = lines.value.find(line => line.startsWith('{time:')) || '';
  return timeLine.replace('{time:', '').replace('}', '');
}

function computeTitle() {
  const titleLine = lines.value.find(line => line.startsWith('{title:')) || '';
  return titleLine.replace('{title:', '').replace('}', '');
}
</script>

<style scoped>
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

.chordchart-body {
  column-count: var(--chordchart-columns);
}

.chordchart-body-stanza {
  break-inside: avoid;
  padding-top: 1em;
}
</style>
