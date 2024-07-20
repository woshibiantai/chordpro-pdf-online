<template>
  <p
    v-if="isLyricsOnly"
    class="chordchart-lyrics"
  >
    {{ props.line }}
  </p>
  <p v-else-if="isComment">
    <span
      v-for="(comment, index) in comments"
      :key="index"
      :class="{
        'chordchart-comment': comment.type === 'comment',
        'chordchart-keychange': comment.type === 'keychange',
      }"
    >
      <template v-if="comment.type === 'keychange'">
        Key - 
      </template>
      {{ comment.value }}
    </span>
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
const isComment = computed(() => props.line.includes('{'));
const isLyricsOnly = computed(() => !isComment.value && !props.line.includes('['));

const comments = computed(() => {
  if (!isComment.value) {
    return [];
  }
  return props.line.split('}')
    .filter((segment) => segment.trim().length > 0)
    .map((segment) => {
      const type = segment.replace(/{|(:.*)/g, '');
      return {
        type,
        value: segment.replace(`{${type}:`, ''),
      };
    });
});

const chordLyricPairs = computed(() => {
  const segments = props.line
    .split(/(\[[\w/#()]*\])/); // Split by chords
  return segments.reduce((chordLine, segment, index) => {
    if (!segment.includes('[')) {
      if (index === 0) {
        chordLine.push({
          chord: '',
          lyric: segment,
        });
      } else {
        return chordLine;
      }
    } else {
      const chord = segment.replace('[', '').replace(']', '');
      const nextSegment = segments[index + 1] || '';
      if (nextSegment.includes('[')) {
        chordLine.push({
          chord,
          lyric: '',
        });
      } else {
        const positionOfFirstSpace = nextSegment.indexOf(' ');
        const firstWord = nextSegment.slice(0, positionOfFirstSpace);
        const followingWords = nextSegment.slice(positionOfFirstSpace + 1);
        chordLine.push({
          chord,
          lyric: firstWord,
        });
        if (followingWords) {
          chordLine.push({
            chord: '',
            lyric: followingWords,
          });
        }
      }
    }
    return chordLine;
  }, []);
});
</script>

<style scoped>
.chordchart-comment {
  font-size: 1em;
  font-weight: bold;
  line-height: 2;
}

.chordchart-keychange {
  float: right;
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
  flex-wrap: wrap;
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
  white-space: pre-wrap;
}

.chordchart-pair-lyric {
  height: 1.1em;
}
</style>
