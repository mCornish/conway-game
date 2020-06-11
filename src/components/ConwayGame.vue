<template>
<div class="container">
  <div class="size-control">
    <label for="size-width">Width:</label>
    <input
      id="size-width"
      @change="(e) => updateGrid(e.target.value, this.height)"
      v-model.number="width"
      type="number"
    />
    <label for="size-height">Height: </label>
    <input
      id="size-height"
      @change="(e) => updateGrid(this.width, e.target.value)"
      v-model.number="height"
      type="number"
    />
  </div>

  <div class="grid">
    <div
      v-for="(cellRow, row) in cells"
      :key="row"
      class="row"
    >
      <Cell
        v-for="(isAlive, column) in cellRow"
        :key="column"
        :is-alive="!!isAlive"
        @click.native="() => updateCell(row, column, !isAlive)"
      />
    </div>
  </div>

  <div class="buttons">
    <button @click="nextGeneration">Next</button>
    <button @click="clear">Clear</button>
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
  display: flex;
  flex-direction: column;

  > * + * {
    margin-top: .2em;
  }
}
.row {
  display: flex;

  > * + * {
    margin-left: .2em;
  }
}
</style>
