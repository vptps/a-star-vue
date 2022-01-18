<template>
  <app-menu
    v-model:gridSize="gridSize"
    v-model:speed="speed"
    v-model:cellSize="cellSize"
  /> <!-- replaces v2's v-bind.sync  -->
  <grid
    :gridSize="gridSize"
    :cellSize="cellSize"
  />
</template>

<script>
import Grid from '@/components/Grid.vue';
import AppMenu from '@/components/AppMenu.vue';

export default {
  name: 'App',

  data() {
    return {
      gridSize: 10,
      speed: 1,
      cellSize: 30
    };
  },

  components: {
    Grid,
    AppMenu
  }
};
</script>

<style>
</style>
