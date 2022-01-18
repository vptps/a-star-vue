<template>
  <div>
    <span>AppMenu</span><br />
    <span>Grid size:</span>
    <input
      type="number"
      :min="$options.MIN_GRID_SIZE"
      :max="$options.MAX_GRID_SIZE"
      step="1"
      :value="gridSize"
      @input="update('gridSize', $event)"
    /><br />

    <span>Speed</span>
    <input
      type="number"
      :min="$options.MIN_SPEED"
      :max="$options.MAX_SPEED"
      step="1"
      :value="speed"
      @input="update('speed', $event)"
    /><br />

    <span>Cell size</span>
    <input
      type="number"
      :min="$options.MIN_CELL_SIZE"
      :max="$options.MAX_CELL_SIZE"
      step="1"
      :value="cellSize"
      @input="update('cellSize', $event)"
    />
  </div>
</template>

<script>
export default {
  name: 'AppMenu',

  MIN_GRID_SIZE: 5, 
  MAX_GRID_SIZE: 60,
  MIN_SPEED: 1,
  MAX_SPEED: 10,
  MIN_CELL_SIZE: 20,
  MAX_CELL_SIZE: 100,

  props: {
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
  },

  methods: {
    getConstFromVariable (variable) {
      switch(variable) {
        case 'gridSize': return 'GRID_SIZE';
        case 'speed': return 'SPEED';
        case 'cellSize': return 'CELL_SIZE';
        default: return 0;
      }
    },

    update (variable, event) {
      let val = parseInt(event.target.value);
      const CONST = this.getConstFromVariable(variable);

      if (isNaN(val)) {
        val = this.$options[`MIN_${CONST}`]
      }
      if (val > this.$options[`MAX_${CONST}`]) {
        val = this.$options[`MAX_${CONST}`];
      }
      if (val < this.$options[`MIN_${CONST}`]) {
        val = this.$options[`MIN_${CONST}`];
      }
      this.$emit('update:' + variable, val);
    },
  },
};
</script>

<style>

</style>