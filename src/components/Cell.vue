<template>
  <div
    draggable="false"
    @mouseover="setCrossable(editBlock)"
    @mousedown="setCrossable(true)"
    :style="cellStyle"
    class="cell"
  >
    <span v-show="showCoordinates">
      {{ cell.x }} {{ cell.y }}
    </span>
  </div>
</template>

<script setup>
import { defineProps, computed, defineEmits, inject } from 'vue';

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
    : props.cell.crossable ? '#e8fdee'
    : 'black'
  };
});

const emit = defineEmits(['toggleCrossable']);

function setCrossable (editBlock) {
  if (editBlock) {
    emit('toggleCrossable', {'x': props.cell.x, 'y': props.cell.y});
  }
}

// here we use a non-mandatory default value in case data was not provided
const showCoordinates = inject('showCoordinates', false);
</script>

<style>
.cell {
  font-size: 6px;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}
</style>