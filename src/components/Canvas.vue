<template>
  <div id="canvas"
    @click="addDiv"
    @mousedown="onMousedown"
    @mouseup="onMouseup"
    @mousemove="onMousemove">
    <RectItem v-for="(item, index) in items"
      :key="index"
      v-bind="item"
      @select="onSelect"
      @move="onMove"
      @resize="onResize"
      />
  </div>
</template>

<script>
import Rect from './Rect.vue';

export default {
  name: "Canvas",
  components: {
    RectItem: Rect,
  },
  props: {

  },
  data() {
    return {
      items: [],
      moveLastX: 0,
      moveLastY: 0,
      mousedown: false,
      selected: null,
    };
  },
  created: function() {
    window.addEventListener('keydown',this.onKeydown);
  },
  destroyed: function() {
    window.removeEventListener('keydown', this.onKeydown);
  },
  methods: {
    addDiv() {
      this.items.push({
        id: Math.random() + "",
        selected: false,
        left: 100 * this.items.length,
        top: 100,
        width: 100,
        height: 100,
      });
    },
    onMove(id, x, y) {
      const item = this.items.filter(i => i.id === id)[0];
      item.left += x;
      item.top += y;
    },
    onResize(id, x, y) {
      const item = this.items.filter(i => i.id === id)[0];
      item.width += x;
      item.height += y;
    },
    onSelect(id) {
      if (this.selected) {
        this.selected.selected = false;
      }
      const item = this.items.filter(i => i.id === id)[0];
      item.selected = true;
      this.selected = item;
    },
    onMousedown(ev) {
      this.moveLastX = ev.clientX;
      this.moveLastY = ev.clientY;
      this.mousedown = true;
    },
    onMouseup(ev) {
      this.mousedown = false;
    },
    onMousemove(ev) {
      if (!this.mousedown || !this.selected) {
        return;
      }
      this.selected.left += ev.clientX - this.moveLastX;
      this.selected.top += ev.clientY - this.moveLastY;
      this.moveLastX = ev.clientX;
      this.moveLastY = ev.clientY;
    },
    onKeydown(ev) {
      if (ev.key === 'Delete') {
        const index = this.items.indexOf(this.selected);
        this.items.splice(index, 1);
      }
    }
  },
};
</script>

<style scoped lang="scss">
#canvas {
  height: 95vh;
  width: 100%;
  position: relative;
}
</style>
