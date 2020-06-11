<template>
  <div
    :class="`cell ${isAlive ? 'is-alive' : ''}`"
  ></div>
</template>

<script>
export default {
  name: "Cell",
  props: {
    isAlive: {
      type: Boolean,
      default: false
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
$color: #390f70;
.cell {
  width: 1em;
  height: 1em;
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
