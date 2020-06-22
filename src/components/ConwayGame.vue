<template>
<div class="container">
  <div class="size-control">
    <label for="size-control">Size: </label>
    <input
      id="size-control"
      v-model.number="size"
      type="number"
      max="100"
      min="0"
    />
  </div>

  <MatrixGrid
    :matrix="cells"
    :on-click="updateCell"
  />

  <div class="buttons">
    <button @click="nextGeneration">Next</button>
    <button @click="clear">Clear</button>
    <button @click="random">Random</button>
    <button
      @click="isPlaying ? stop() : play()"
    >{{isPlaying ? 'Stop' : 'Play'}}</button>
  </div>
</div>
</template>

<script>
import { debounce as _debounce } from 'lodash';
import MatrixGrid from './MatrixGrid';
import { Generation, nextGeneration } from '../game';

export default {
  name: 'ConwayGame',
  components: { MatrixGrid },
  props: {
    initialSize: {
      type: Number,
      default: 10
    }
  },
  data() {
    return {
      cells: [],
      isPlaying: false,
      size: this.initialSize,
    };
  },
  watch: {
    size(newSize) {
      console.log("size -> newSize", Number(newSize))
      this.setGridSize(Number(newSize))
    }
  },
  created() {
    this.cells = Generation(this.size, this.size);
  },
  methods: {
    clear() {
      this.isPlaying = false;
      this.cells = Generation(this.size, this.size);
    },
    nextGeneration() {
      this.isPlaying = false;
      this.cells = nextGeneration(this.cells);
    },
    play() {
      this.isPlaying = true;
      this.playLoop();
    },
    playLoop() {
      if (!this.isPlaying) return undefined;
      this.cells = nextGeneration(this.cells);
      setTimeout(this.playLoop, 1000);
    },
    random() {
      this.cells = this.cells.map(cellRow => cellRow.map(randomIsAlive))
    },
    setGridSize: _debounce(function(size) {
      this.updateGrid(size);
      this.size = size;
    }, 200),
    stop() {
      this.isPlaying = false;
    },
    updateCell(row, column, isAlive) {
      let newCells = [...this.cells];
      newCells[row][column] = Number(isAlive);
      this.cells = newCells;
    },
    updateGrid(size) {
      this.cells = Generation(size, size);
    }
  }
};

function randomIsAlive() {
  return Number(Math.random() > .5);
}
</script>

<style scoped lang="scss">
.container {
  display: flex;
  flex-direction: column;
  align-items: center;

  > * + * {
    margin-top: 1em;
  }
}
.size-control,
.buttons {
  > * + * {
    margin-left: 1em;
  }
}
</style>
