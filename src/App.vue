<template>
  <div class="flex-container">
    <app-menu
      v-model:gridSize="gridSize"
      v-model:speed="speed"
      v-model:cellSize="cellSize"
      @startAlgo="startAlgo"
      @resetGrid="create2dArray"
    /> <!-- replaces v2's v-bind.sync  -->
  </div>
  <div class="flex-container">
    <grid
      :gridSize="gridSize"
      :cellSize="cellSize"
      :cellList="cellList"
      @toggleCrossable="toggleCrossable($event)"
    />
  </div>
</template>

<!-- Component name's registration is based on SFC name, no need to explicitely declare it -->
<script setup>
import Grid from '@/components/Grid.vue';
import AppMenu from '@/components/AppMenu.vue';

import { ref, onMounted, watch } from 'vue';
import { showSolution, getNodeNeighbours, getDistance, compareNodes, sleep } from '@/utils/utils.js';


const gridSize = ref(15);
const speed = ref(1);
const cellSize = ref(30);
const cellList = ref([]);

function create2dArray () {
  cellList.value = [];
  for (let i = 0; i < gridSize.value; i++) {
    cellList.value.push([]);
  }

  cellList.value.forEach((row, index) => {
    for (let j = 0; j < gridSize.value; j++) {
      row.push({
        'x': index,
        'y': j,
        'start': false,
        'end': false,
        'cost': 0,
        'heuristique': 0,
        'crossable': true,
        'parent': undefined,
        'current': false,
        'neighbour': false,
        'closed': false,
        'solution': false
      });
    }
  });

  // set default start and end position
  cellList.value[1][gridSize.value - 2].end = true;
  cellList.value[gridSize.value - 2][1].start = true;
}

function toggleCrossable (event) {
  let cell = cellList.value[event.x][event.y]
  cell.crossable = !cell.crossable;
}

// Start A* algorithm
async function startAlgo () {
  // find start and end node
  let start = cellList.value[gridSize.value - 2][1];
  let end = cellList.value[1][gridSize.value - 2];

  let closedList = [];
  let openList = [];
  openList.push(start);

  while (openList.length > 0) {
    await sleep(speed.value);
    let u = openList.pop();
    u.current = true;

    if (u.x === end.x && u.y === end.y) {
      showSolution(u);
      break;
    }

    let neighbours = getNodeNeighbours(u, gridSize.value, cellList .value);
    neighbours.forEach(v => {
      v.neighbour = true;

      if (!v.crossable) {
        closedList.push(v);
      }

      if (!(closedList.includes(v) || (openList.includes(v) && v.cost < u.cost + 1))) {
        v.parent = u;
        v.cost = u.cost + 1;
        v.heuristique = v.cost + getDistance(v, end);
        if (openList.includes(v)) {
          const idx = openList.indexOf(v);
          openList.splice(idx, 1);
        }
        openList.push(v);
        openList.sort(compareNodes);
      }
    })

    closedList.push(u);
    await sleep(speed.value);
    u.current = false;
    u.closed = true;
  }
}

watch(gridSize, () => {
  // reinit on size change
  create2dArray();
});

onMounted(() => {
  create2dArray();
})
</script>

<style>
html, body {
  height: 100%;
}

body {
  margin: 0;
  background-color: #e8fdee;
  font-family: system-ui;
}

.flex-container {
  height: 100%;
  padding: 15px;
  margin: 0;
  display: -webkit-box;
  display: -moz-box;
  display: -ms-flexbox;
  display: -webkit-flex;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
