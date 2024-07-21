<template>
  <menu>
    <div class="field">
      <label for="fontSizeInput">Font size:</label>
      <input
        id="fontSizeInput"
        min="1"
        type="number"
        :value="fontSize"
        @input="onFontSizeInput"
      />
    </div>
    <div class="field">
      <label for="columnSelect">Number of columns:</label>
      <select
        id="columnSelect"
        name="Columns"
        @change="updateColumns"
      >
        <option
          value="1"
          default
        >
          One
        </option>
        <option
          value="2"
        >
          Two
        </option>
      </select>
      </div>
    <button
      @click="onPrint()"
      type="button"
    >
      Print
    </button>
  </menu>
</template>

<script setup>
const columns = defineModel('columns', { type: Number, default: 1 });
const fontSize = defineModel('fontSize', { type: Number, default: 16 });

function onPrint() {
  window.print();
}

function updateColumns(event) {
  columns.value = Number(event.target.value);
}

function onFontSizeInput(event) {
  const value = Number(event.target.value);
  if (value > 0) {
    fontSize.value = value;
  }
}
</script>

<style scoped>
menu {
  display: flex;
  gap: 16px;
  padding-bottom: 8px;
  padding-left: 0;
  padding-top: 8px;
  padding-right: 0;
}

.field {
  display: flex;
  gap: 8px;
}
</style>
