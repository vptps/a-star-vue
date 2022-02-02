<template>
  <app-menu
    v-model:gridSize="gridSize"
    v-model:speed="speed"
    v-model:cellSize="cellSize"
    @startAlgo="startAlgo"
  /> <!-- replaces v2's v-bind.sync  -->
  <grid
    :gridSize="gridSize"
    :cellSize="cellSize"
    :cellList="cellList"
    @toggleCrossable="toggleCrossable($event)"
  />
</template>

<script>
import Grid from '@/components/Grid.vue';
import AppMenu from '@/components/AppMenu.vue';

export default {
  name: 'App',

  data() {
    return {
      gridSize: 15,
      speed: 1,
      cellSize: 30,
      cellList: []
    };
  },

  components: {
    Grid,
    AppMenu
  },

  methods: {
    create2dArray () {
      this.cellList = [];
      for (let i = 0; i < this.gridSize; i++) {
        this.cellList.push([]);
      }

      this.cellList.forEach((row, index) => {
        for (let j = 0; j < this.gridSize; j++) {
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
      this.cellList[1][this.gridSize - 2].end = true;
      this.cellList[this.gridSize - 2][1].start = true;
    },

    toggleCrossable (event) {
      let cell = this.cellList[event.x][event.y]
      cell.crossable = !cell.crossable;
    },

    // Start A* algorithm
    async startAlgo () {
      // find start and end node
      let start = this.cellList[this.gridSize - 2][1];
      let end = this.cellList[1][this.gridSize - 2];

      let closedList = [];
      let openList = [];
      openList.push(start);

      while (openList.length > 0) {
        await this.sleep();
        let u = openList.pop();
        u.current = true;

        if (u.x === end.x && u.y === end.y) {
          this.showSolution(u);
          break;
        }

        let neighbours = this.getNodeNeighbours(u);
        neighbours.forEach(v => {
          v.neighbour = true;

          if (!v.crossable) {
            closedList.push(v);
          }

          if (!(closedList.includes(v) || (openList.includes(v) && v.cost < u.cost + 1))) {
            v.parent = u;
            v.cost = u.cost + 1;
            v.heuristique = v.cost + this.getDistance(v, end);
            if (openList.includes(v)) {
              const idx = openList.indexOf(v);
              openList.splice(idx, 1);
            }
            openList.push(v);
            openList.sort(this.compareNodes);
          }
        })

        closedList.push(u);
        await this.sleep();
        u.current = false;
        u.closed = true;
      }
    },

    /************************
     * Utils methods
     * **********************/
    async showSolution(node) {
      let parent = node.parent;
      while (parent !== undefined) {
        await this.sleep();
        parent.solution = true;
        parent = parent.parent;
      }
    },

    getNodeNeighbours(node) {
      const [x, y] = [node.x, node.y];
      let nodeToAdd = undefined;
      let neighbours = [];

      for (let i=x-1; i<=x+1; i++) {
        for (let j=y-1; j<=y+1; j++) {
          if (i===x && j===y) continue;
          if (i<0 || j<0) continue; // TODO: add outer bounds
          if (i>=this.gridSize || j >= this.gridSize) continue;

          console.log(i, j)
          nodeToAdd = this.cellList[i][j];
          if (nodeToAdd) neighbours.push(nodeToAdd);
        }
      }

      return neighbours;
    },

    getDistance(nodeA, nodeB) {
      return Math.sqrt(Math.pow(nodeB.x - nodeA.x, 2) + Math.pow(nodeB.y - nodeA.y, 2));
    },

    compareNodes(nodeA, nodeB) {
      if (nodeA.heuristique < nodeB.heuristique) {
        return 1;
      } else if (nodeA.heuristique === nodeB.heuristique) {
        return 0;
      } else {
        return -1;
      }
    },

    sleep() {
      return new Promise(resolve => setTimeout(resolve, 75 / this.speed));
    }
  },

  watch: {
    gridSize () {
      // reinit on size change
      this.create2dArray();
    }
  },

  mounted () {
    this.create2dArray();
  }
};
</script>

<style>
</style>
