<template>
  <div id="canvas"
    @mousedown="onMousedown"
    @mouseup="onMouseup"
    @mousemove="onMousemove">
    <RectItem v-for="(item, index) in items"
      :key="index"
      v-bind="item"
      :zoom="zoom"
      @select="onSelect"
      @drag="onDrag"
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
      zoom: 1,
    };
  },
  created: function() {
    window.addEventListener('keydown',this.onKeydown);
  },
  destroyed: function() {
    window.removeEventListener('keydown', this.onKeydown);
  },
  methods: {
    addDiv({ left, top, width, height }) {
      const item = {
        id: Math.random() + "",
        selected: false,
        left: left || 100 * this.items.length,
        top: top || 100,
        width: width || 100,
        height: height || 100,
        isBeingDrawn: true,
      };
      this.items.push(item);
      return item;
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
    onDrag(id) {
      if (this.dragging) {
        this.dragging = null;
      }
      console.log('onDrag');
      const item = this.items.filter(i => i.id === id)[0];
      this.dragging = item;
    },
    onMousedown(ev) {
      console.log(ev);
      const { layerX: x, layerY: y, clientX, clientY } = ev;
      this.moveLastX = clientX;
      this.moveLastY = clientY;
      this.mousedown = true;

      if (this.selected) {
        this.selected.selected = false;
        this.selected = null;
      } else {
        this.drawing = this.addDiv({ left: x, top: y, width: 1, height: 1 });
      }
    },
    onMouseup(ev) {
      this.mousedown = false;
      this.drawing = null;
      this.dragging = null;
    },
    onMousemove(ev) {
      if (!this.mousedown || (!this.dragging && !this.drawing)) {
        return;
      }
      if (this.dragging) {
        this.dragging.left += ev.clientX - this.moveLastX;
        this.dragging.top += ev.clientY - this.moveLastY;
      } else {
        this.drawing.width += ev.clientX - this.moveLastX;
        this.drawing.height += ev.clientY - this.moveLastY;
      }
      this.moveLastX = ev.clientX;
      this.moveLastY = ev.clientY;
    },
    onKeydown(ev) {
      if (ev.key === 'Delete') {
        const index = this.items.indexOf(this.selected);
        this.items.splice(index, 1);
      }
      if (ev.key === '+' || ev.key === '=') {
        this.zoom += 0.1;
      }
      if (ev.key === '-') {
        this.zoom -= 0.1;
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
