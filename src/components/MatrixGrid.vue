<template>
<div
  class="grid"
  :style="{
    gridTemplateRows: `repeat(${height}, 1fr)`,
    gridTemplateColumns: `repeat(${width}, 1fr)`,
    gridGap: `${5 / Math.min(width, height)}em`
  }"
>
  <template
    v-for="(cellRow, row) in matrix"
  >
    <MatrixCell
      v-for="(isAlive, column) in cellRow"
      :key="`${row}-${column}`"
      :grid-width="Number(width)"
      :grid-height="Number(height)"
      :is-alive="!!isAlive"
      @click.native="() => updateCell(row, column, !isAlive)"
    />
  </template>
</div>
</template>

<script>
import MatrixCell from './MatrixCell';

export default {
  name: 'Grid',
  components: { MatrixCell },
  props: {
    matrix: {
      type: Array,
      required: true,
      default: () => [],
      validator(value) {
        return value.every(row => row.every(n => n === 0 || n === 1));
      }
    }
  },
  computed: {
    height() {
      return this.matrix.length;
    },
    width() {
      return this.matrix.length ? this.matrix[0].length : 0;
    }
  }
};
</script>

<style scoped lang="scss">
.grid {
  display: grid;
  width: 100%;
  height: 100%;
}
</style>
