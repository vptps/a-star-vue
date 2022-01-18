<template>
  <div class="cell" :style="cellStyle">
    
  </div>
</template>

<script>
export default {
  name: 'Cell',

  props: {
    cellSize: {
      type: Number,
      required: true
    }
  },

  computed: {
    cellStyle () {
      return {
        'height': `${this.cellSize}px`,
        'width': `${this.cellSize}px`
      };
    }
  }
};
</script>

<style>
  
</style>