<template>
  <div
    id="grid"
    v-if="cellList.length"
  >
    <div
      v-for="i in gridSize"
      :key="'row' + i"
      class="row"
      @mousedown="editBlock = true"
      @mouseup="editBlock = false"
    >
      <cell
        v-for="j in gridSize"
        :key="'col' + j + '_' + i"
        :cellSize="cellSize"
        :cell="cellList[i - 1][j - 1]"
        :editBlock="editBlock"
        @toggleCrossable="reemit"
      />
    </div>
  </div>
</template>

<script setup>
import Cell from '@/components/Cell.vue';

import { ref, defineProps, defineEmits } from 'vue';

const editBlock = ref(false)

defineProps({
  gridSize: {
    type: Number,
    required: true
  },
  cellSize: {
    type: Number,
    required: true
  },
  cellList: {
    type: Array,
    required: true
  }
});

const emit = defineEmits(['toggleCrossable']);

function reemit (evt) {
  emit('toggleCrossable', evt);
}
</script>

<style>
#grid {
  width: min-content;
}
.row {
  width: max-content;
}
.cell {
  border: solid 1px black;
  float: left;
}
</style>
