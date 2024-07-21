<template>
  <span class="chordchart-chord-lyrics-pair">
    <code class="chordchart-chords">
      {{ pair.chords }}
    </code>
    <span class="chordchart-lyrics">
      {{ pair.lyrics }}
    </span>
  </span>
</template>

<script setup>
import { ChordLyricsPair } from 'chordsheetjs';

defineProps({
  pair: {
    type: [ChordLyricsPair],
    required: true,
  }
});
</script>

<style scoped>
.chordchart-chord-lyrics-pair {
  align-items: flex-start;
  display: inline-flex;
  flex-direction: column;
  margin-right: .4em;
  margin-top: .2em;

  &:first-of-type {
    margin-left: 2em;
  }
}

.chordchart-chords {
  font-family: inherit;
  font-size: 1em;
  font-weight: bold;
  height: 1.1em;
  line-height: 1.1em;
  padding-right: .4em;
  white-space: pre-wrap;
}

.chordchart-lyrics {
  font-size: 1em;
  height: 1.1em;
  line-height: 1.1em;
}
</style>
