<template>
<div class="container">
  <div class="size-control">
    <!-- <label for="size-width">Width:</label>
    <input
      id="size-width"
      @change="(e) => setGridSize(e.target.value, height)"
      :value="width"
      type="number"
      max="100"
      min="0"
    /> -->
    <label for="size-height">Size: </label>
    <input
      id="size-height"
      @change="(e) => setGridSize(e.target.value, e.target.value)"
      :value="height"
      type="number"
      max="100"
      min="0"
    />
  </div>

  <div class="grid-container" ref="grid">
    <div
      class="grid"
      :style="{
        gridTemplateRows: `repeat(${cells.length}, 1fr)`,
        gridTemplateColumns: `repeat(${cells[0].length}, 1fr)`,
        gridGap: `${5 / Math.min(width, height)}em`
      }"
    >
      <template
        v-for="(cellRow, row) in cells"
      >
        <Cell
          v-for="(isAlive, column) in cellRow"
          :key="`${row}-${column}`"
          :grid-width="Number(width)"
          :grid-height="Number(height)"
          :is-alive="!!isAlive"
          @click.native="() => updateCell(row, column, !isAlive)"
        />
      </template>
    </div>
  </div>

  <div class="buttons">
    <button @click="nextGeneration">Next</button>
    <button @click="clear">Clear</button>
    <button @click="random">Random</button>
    <button @click="play">Play</button>
    <button @click="stop">Stop</button>
  </div>
</div>
</template>

<script>
import Cell from './Cell';
import { Generation, nextGeneration } from '../game';

export default {
  name: "ConwayGame",
  components: { Cell },
  props: {
    initialWidth: {
      type: Number,
      default: 10
    },
    initialHeight: {
      type: Number,
      default: 10
    }
  },
  data() {
    return {
      cells: [],
      height: this.initialHeight,
      isPlaying: false,
      width: this.initialWidth,
    };
  },
  created() {
    this.cells = Generation(this.width, this.height);
  },
  methods: {
    clear() {
      this.cells = Generation(this.width, this.height);
    },
    nextGeneration() {
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
    setGridSize(width, height) {
      this.updateGrid(width, height);
      this.width = width;
      this.height = height;
    },
    stop() {
      this.isPlaying = false;
    },
    updateCell(row, column, isAlive) {
      let newCells = [...this.cells];
      newCells[row][column] = Number(isAlive);
      this.cells = newCells;
    },
    updateGrid(width, height) {
      this.cells = Generation(width, height);
    }
  }
};

function randomIsAlive() {
  return Number(Math.random() > .5);
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
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
.grid {
  display: grid;
  width: 100%;
  height: 100%;
}
.row {
  display: flex;
  width: 100%;
}
</style>
