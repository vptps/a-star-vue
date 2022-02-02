<template>
    <div>Grid</div>
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
          :key="'col' + j"
          :cellSize="cellSize"
          :cell="cellList[i - 1][j - 1]"
          :editBlock="editBlock"
          @toggleCrossable="reemit($event)"
        />
      </div>
    </div>
</template>

<script>
import Cell from '@/components/Cell.vue';

export default {
  name: 'Grid',

  data() {
    return {
      editBlock: false
    };
  },

  props: {
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
  },

  components: {
    Cell
  },

  methods: {
    reemit (event) {
      this.$emit('toggleCrossable', event);
    }
  },

  emits: ['toggleCrossable']
};
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
