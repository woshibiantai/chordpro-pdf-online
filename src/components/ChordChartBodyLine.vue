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
      <div class="chordchart-pair-chord">{{ pair.chord }}</div>
      <div class="chordchart-pair-lyric">{{ pair.lyric }}</div>
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
  font-weight: bold;
  line-height: 1.3em;
  margin-bottom: .8em;
}

.chordchart-lyrics {
  line-height: 1.3em;
  margin-bottom: 0;
}

.chordchart-line {
  display: flex;
  gap: .4em;
  margin-bottom: .8em;
  margin-top: 0;
}

.chordchart-pair {
  align-items: flex-start;
  display: inline-flex;
  flex-direction: column;
}

.chordchart-pair-chord {
  font-weight: bold;
  height: 1.1em;
  line-height: 1.1em;
}

.chordchart-pair-lyric {
  height: 1.1em;
  line-height: 1.1em;
}
</style>
