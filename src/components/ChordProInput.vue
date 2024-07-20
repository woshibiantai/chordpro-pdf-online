<template>
  <form>
    <label for="chordpro-input">Chord chart in ChordPro format:</label>
    <textarea
      v-model="input"
      id="chordpro-input"
      rows="10"
    />
  </form>
</template>

<script setup>
const input = defineModel('input', { type: String, default: '' });
</script>

<style scoped>
label {
  display: block;
  font-size: 12px;
  font-weight: bold;
  margin-bottom: 4px;
}

textarea {
  font-family: monospace;
  font-size: 14px;
  width: 100%;
}
</style>
