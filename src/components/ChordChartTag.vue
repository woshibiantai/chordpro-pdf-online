<template>
  <span
    v-if="isKeychange"
    class="chordchart-tag chordchart-keychange"
  >
    Key - {{ tag.value }}
  </span>
  <span
    v-else
    class="chordchart-tag"
  >
    {{ tag.value }}
  </span>
</template>

<script setup>
import { Tag } from 'chordsheetjs';
const props = defineProps({
  tag: {
    type: Tag,
    required: true,
  }
});

const isKeychange = props.tag.name === 'keychange';
</script>

<style scoped>
.chordchart-tag {
  font-size: 1em;
  font-weight: bold;
  line-height: 2;
}

.chordchart-keychange {
  display: block;
  flex: 1;
  text-align: right;
}
</style>
