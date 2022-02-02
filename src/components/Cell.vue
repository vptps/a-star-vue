<template>
  <div
    @mouseover="setCrossable"
    @mousedown="test"
    :style="cellStyle"
    class="cell"
  >
  </div>
</template>

<script>
export default {
  name: 'Cell',

  props: {
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
  },

  computed: {
    cellStyle () {
      return {
        'height': `${this.cellSize}px`,
        'width': `${this.cellSize}px`,
        'background-color': this.cell.start ? 'green' : this.cell.end ? 'red' : this.cell.current ? 'yellow' : this.cell.solution ? 'purple' : this.cell.closed ? 'lightblue' : this.cell.crossable ? 'white' : 'black'
      };
    }
  },

  methods: {
    test () {
      console.log('test', this.cell.x, this.cell.y)
      this.$emit('toggleCrossable', {'x': this.cell.x, 'y': this.cell.y});
    },

    setCrossable () {
      if (this.editBlock) {
        this.$emit('toggleCrossable', {'x': this.cell.x, 'y': this.cell.y});
      }
    }
  },

  emits: ['toggleCrossable']
};
</script>

<style>
  
</style>