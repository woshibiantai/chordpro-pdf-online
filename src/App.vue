<template>
  <RouterView />
</template>

<script setup>
import { provide, ref } from 'vue';
import { RouterView } from 'vue-router'

const searchParams = new URLSearchParams(location.search);
const googleDriveFileId = searchParams.get('googleDriveFileId');
const chordProInput = ref('');
const isLoading = ref(true);
provide('chordProInput', chordProInput);
provide('isLoading', isLoading);

if (googleDriveFileId) {
  fetch(`https://www.googleapis.com/drive/v3/files/${googleDriveFileId}?alt=media`, {
    method: 'GET',
    headers: {
      'X-goog-api-key': import.meta.env.VITE_GOOGLE_API_KEY,
    }
  })
    .then(response => {
      if (!response.ok) {
        throw new Error('Network response was not ok');
      }
      return response;
    })
    .then(response => response.text())
    .then(text => {
      chordProInput.value = text;
      isLoading.value = false;
    })
    .catch(() => {
      setSampleInput();
    });
} else {
  setSampleInput();
}

function setSampleInput() {
  import('@/assets/sample-chart.js')
    .then(({ sampleChordProChart }) => {
      chordProInput.value = sampleChordProChart;
      isLoading.value = false;
    });
}
</script>

<style scoped>
nav {
  display: flex;
  justify-content: center;
  gap: 1rem;
  padding: 1rem;
  background-color: #f0f0f0;
}
</style>
