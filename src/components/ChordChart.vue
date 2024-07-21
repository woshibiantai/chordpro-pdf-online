<template>
  <section class="chordchart">
    <h1>{{ title }}</h1>
    <p
      v-if="artist"
      class="chordchart-artist"
    >{{ artist }}</p>
    <p
      v-if="key || tempo || time"
      class="chordchart-metadata-line"
    >
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
      <ChordChartBodyParagraph
        v-for="(paragraph, index) in bodyParagraphs"
        :key="index"
        :paragraph="paragraph"
      />
    </div>
    <footer v-if="copyright">
      <p>{{ copyright }}</p>
    </footer>
  </section>
</template>

<script setup>
import { computed, inject, ref, watch } from 'vue';
import { ChordProParser, Song } from 'chordsheetjs';
import ChordChartBodyParagraph from './ChordChartBodyParagraph.vue';

defineProps({
  columns: {
    type: Number,
    default: 1,
  },
});


const chordProInput = inject('chordProInput');
const parser = new ChordProParser();
const song = ref(new Song());
const metadata = computed(() => song.value.metadata);

const artist = computed(() => metadata.value.get('artist'));
const bodyParagraphs = computed(() => song.value.bodyParagraphs);
const copyright = computed(() => metadata.value.get('copyright'));
const key = computed(() => metadata.value.get('key'));
const tempo = computed(() => metadata.value.get('tempo'));
const time = computed(() => metadata.value.get('time'));
const title = computed(() => metadata.value.get('title'));

watch(chordProInput, (newValue) => {
  try {
    song.value = parser.parse(newValue);
  } catch(err) {
    console.error(err);
  }
}, { immediate: true });
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

footer {
  font-size: .7em;
  margin-top: 1em;
  text-align: center;
}
</style>
