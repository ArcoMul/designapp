<template>
  <div 
    @mousedown.stop.prevent="onMousedown">
  </div>
</template>

<script>
export default {
  name: "Dragable",
  components: {
  },
  props: {
  },
  data() {
    return {
      moveLastX: 0,
      moveLastY: 0,
      mousedown: false,
    };
  },
  created: function() {
    window.addEventListener('mousemove',this.onMousemove);
    window.addEventListener('mouseup',this.onMouseup);
  },
  destroyed: function() {
    window.removeEventListener('mousemove', this.onMousemove);
    window.removeEventListener('mouseup', this.onMouseup);
  },
  methods: {
    onMousedown(ev) {
      this.moveLastX = ev.clientX;
      this.moveLastY = ev.clientY;
      this.mousedown = true;
    },
    onMouseup(ev) {
      this.mousedown = false;
    },
    onMousemove(ev) {
      if (!this.mousedown) {
        return;
      }
      this.$emit('move', ev.clientX - this.moveLastX, ev.clientY - this.moveLastY)
      this.moveLastX = ev.clientX;
      this.moveLastY = ev.clientY;
    },
  },
};
</script>

<style scoped lang="scss">
</style>
