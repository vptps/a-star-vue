<template>
  <div>
    <label>Grid size</label>
    <input
      type="number"
      :min="customOptions.MIN_GRID_SIZE"
      :max="customOptions.MAX_GRID_SIZE"
      step="1"
      :value="gridSize"
      @input="update('gridSize', $event)"
    /><br />

    <label>Speed</label>
    <input
      type="number"
      :min="customOptions.MIN_SPEED"
      :max="customOptions.MAX_SPEED"
      step="1"
      :value="speed"
      @input="update('speed', $event)"
    /><br />

    <label>Cell size</label>
    <input
      type="number"
      :min="customOptions.MIN_CELL_SIZE"
      :max="customOptions.MAX_CELL_SIZE"
      step="1"
      :value="cellSize"
      @input="update('cellSize', $event)"
    />

    <br /><br />
    <button @click="$emit('startAlgo')">Start algo</button>
    <button @click="$emit('resetGrid')">Reset</button>
  </div>
</template>

<script setup>
import { defineProps, defineEmits, readonly } from 'vue';

const customOptions = readonly({
  'MIN_GRID_SIZE': 5,
  'MAX_GRID_SIZE': 60,
  'MIN_SPEED': 1,
  'MAX_SPEED': 10,
  'MIN_CELL_SIZE': 20,
  'MAX_CELL_SIZE': 70
});

defineProps({
  gridSize: {
    type: Number,
    required: true
  },
  speed: {
    type: Number,
    required: true
  },
  cellSize: {
    type: Number,
    required: true
  }
});

function getConstFromVariable (variable) {
  switch(variable) {
    case 'gridSize': return 'GRID_SIZE';
    case 'speed': return 'SPEED';
    case 'cellSize': return 'CELL_SIZE';
    default: return 0;
  }
}

function update (variable, event) {
  let val = parseInt(event.target.value);
  const CONST = getConstFromVariable(variable);

  if (isNaN(val)) {
    val = customOptions[`MIN_${CONST}`]
  }
  if (val > customOptions[`MAX_${CONST}`]) {
    val = customOptions[`MAX_${CONST}`];
  }
  if (val < customOptions[`MIN_${CONST}`]) {
    val = customOptions[`MIN_${CONST}`];
  }
  emit('update:' + variable, val);
}

const emit = defineEmits(['update:gridSize', 'update:speed', 'update:cellSize', 'startAlgo', 'resetGrid']);
</script>

<style>
input {
  float: right;
}

label {
  float: left;
}
</style>