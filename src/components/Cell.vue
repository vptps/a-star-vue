<template>
  <div
    draggable="false"
    @mouseover="setCrossable(editBlock)"
    @mousedown="setCrossable(true)"
    :style="cellStyle"
    class="cell"
  >
  </div>
</template>

<script setup>
import { defineProps, computed, defineEmits } from 'vue';

const props = defineProps({
  cellSize: {
    type: Number,
    required: true
  },
  cell: {
    type: Object,
    required: true
  },
  editBlock: {
    type: Boolean,
    required: true
  }
});

const cellStyle = computed(() => {
  return {
    'height': `${props.cellSize}px`,
    'width': `${props.cellSize}px`,
    'background-color': props.cell.start ? 'green'
    : props.cell.end ? 'red'
    : props.cell.current ? 'yellow'
    : props.cell.solution ? 'purple'
    : props.cell.closed ? 'lightblue'
    : props.cell.crossable ? 'white'
    : 'black'
  };
});

const emit = defineEmits(['toggleCrossable']);

function setCrossable (editBlock) {
  if (editBlock) {
    emit('toggleCrossable', {'x': props.cell.x, 'y': props.cell.y});
  }
}
</script>

<style>
  
</style>