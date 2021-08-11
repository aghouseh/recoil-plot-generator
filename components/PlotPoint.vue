<template>
  <span class="PlotPoint" :style="position" />
</template>
<script>
export default {
  name: 'PlotPoint',
  props: {
    x: {
      type: Number,
      default: 0
    },
    y: {
      type: Number,
      default: 0
    }
  },
  computed: {
    position () {
      return {
        left: `${this.x}px`,
        bottom: `${this.y}px`
      }
    }
  }
}
</script>
<style>
.PlotPoint {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background-color: red;
  opacity: 0.5;
  position: absolute;
  /* margin-bottom: -03px; */
  margin-left: -3px;
}
.PlotPoint:hover {
  transform: scale(1.5);
}
</style>
