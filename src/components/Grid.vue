<template>
    <div>Grid</div>
    <div id="grid" style="'background-color: blue'" >
      <div v-for="i in gridSize" :key="'row' + i" class="row">
        <cell 
          v-for="j in gridSize"
          :key="'col' + j"
          :cellSize="cellSize"
          :id="(i=== 2 && j === gridSize -1 ? 'end' : (i=== gridSize - 1 && j === 2 ? 'start' : undefined))"
        />
      </div>
    </div>
</template>

<script>
import Cell from '@/components/Cell.vue';

export default {
  name: 'Grid',

  props: {
    gridSize: {
      type: Number,
      required: true
    },
    cellSize: {
      type: Number,
      required: true
    }
  },

  components: {
    Cell
  }
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
#end {
  background-color: red;
}

#start {
  background-color: green;
}
</style>
