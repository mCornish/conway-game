<template>
  <div
    :class="{
      cell: true,
      'is-alive': isAlive
    }"
    :style="{
      width,
      height,
      borderWidth: `${borderWidth}em`
    }"
  ></div>
</template>

<script>
export default {
  name: 'GridCell',
  props: {
    gridWidth: {
      type: Number,
      required: true
    },
    gridHeight: {
      type: Number,
      required: true
    },
    isAlive: {
      type: Boolean,
      default: false
    },
  },
  data() {
    return {
      isMounted: false,
      // sizeString: '100%',
      width: '100%',
      height: '100%'
    };
  },
  computed: {
    borderWidth() {
      return 5 / Math.max(this.gridWidth, this.gridHeight);
    },
  },
  watch: {
    gridWidth: 'updateWidth',
    gridHeight: 'updateHeight'
  },
  mounted() {
    this.$nextTick(function () {
      this.isMounted = true;
    });
  },
  methods: {
    updateWidth(newGridWidth) {
      if (!this.isMounted) return undefined;

      this.width = newGridWidth < this.gridHeight ?
        `${this.$el.clientHeight}px` : '100%';
    },
    updateHeight(newGridHeight) {
      if (!this.isMounted) return undefined;

      this.height = newGridHeight < this.gridWidth ?
        `${this.$el.clientWidth}px` : '100%';
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
$color: #390f70;
.cell {
  width: 100%;
  height: 100%;
  box-sizing: border-box;
  border: .3em solid $color;
  border-radius: 50%;
  cursor: pointer;
  transition: all .2s ease;
}
.cell:hover,
.is-alive {
  background-color: $color;  
}
</style>
