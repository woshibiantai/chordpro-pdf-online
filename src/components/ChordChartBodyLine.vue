<template>
  <div
    v-if="isLineBreak"
    class="chordchart-linebreak"
  />
  <p
    v-else-if="isLyricsOnly"
    class="chordchart-lyrics"
  >
    {{ props.line }}
  </p>
  <p
    v-else-if="isComment"
    class="chordchart-comment"
  >
    {{ comment }}
  </p>
  <p
    v-else
    class="chordchart-line"
  >
    <span
      v-for="(pair, index) in chordLyricPairs"
      :key="index"
      class="chordchart-pair"
    >
      <code class="chordchart-pair-chord">{{ pair.chord }}</code>
      <span
        v-if="!isChordsOnly"
        class="chordchart-pair-lyric"
      >
        {{ pair.lyric }}
      </span>
    </span>
  </p>
</template>

<script setup>
import { computed } from 'vue';

const props = defineProps({
  line: {
    type: String,
    default: '',
  },
});

const isChordsOnly = computed(() => {
  const withoutChords = props.line.replace(/\[([^[\]]*)\]/g, '');
  return withoutChords.trim().length === 0;
});
const isComment = computed(() => props.line.startsWith('{comment:'));
const isLineBreak = computed(() => props.line.trim() === ''); 
const isLyricsOnly = computed(() => !isComment.value && !props.line.includes('['));

const comment = computed(() => {
  if (!isComment.value) {
    return '';
  }
  return props.line.replace('{comment:', '').replace('}', '');
});

const chordLyricPairs = computed(() => {
  const segments = props.line.split(' ');
  return segments.map((segment) => {
    if (!segment.startsWith('[')) {
      return {
        chord: ' ',
        lyric: segment,
      };
    }
    const withoutOpeningBracket = segment.replace('[', '');
    const [chord, lyric] = withoutOpeningBracket.split(']');
    return {
      chord,
      lyric,
    };
  });
});
</script>

<style scoped>
.chordchart-linebreak {
  margin-bottom: 2em;
}

.chordchart-comment {
  font-size: 1em;
  font-weight: bold;
  line-height: 2;
}

.chordchart-lyrics {
  line-height: 1.3em;
  margin-bottom: 0;
  margin-left: 2em;
}

.chordchart-line {
  display: flex;
  gap: .4em;
  line-height: 1.1em;
  margin-left: 2em;
  margin-top: .2em;
}

.chordchart-pair {
  align-items: flex-start;
  display: inline-flex;
  flex-direction: column;
}

.chordchart-pair-chord {
  font-weight: bold;
  font-family: inherit;
  height: 1.1em;
  padding-right: .4em;
  white-space: nowrap;
}

.chordchart-pair-lyric {
  height: 1.1em;
}
</style>
