<template>
  <div :class="{ item: true, selected }" :style="transpiledStyle" @click.stop.prevent @mousedown="onMousedown">
    <div v-if="selected" class="handles">
      <Dragable class="top" @move="(x, y) => { move(0, y); resize(0, -y) }" />
      <Dragable class="top-right" @move="(x, y) => { move(0, y); resize(x, -y) }" />
      <Dragable class="right" @move="(x, y) => { move(0, 0); resize(x, 0) }" />
      <Dragable class="bottom-right" @move="(x, y) => { move(0, 0); resize(x, y) }" />
      <Dragable class="bottom" @move="(x, y) => { move(0, 0); resize(0, y) }" />
      <Dragable class="bottom-left" @move="(x, y) => { move(x, 0); resize(-x, y) }" />
      <Dragable class="left" @move="(x, y) => { move(x, 0); resize(-x, 0) }" />
      <Dragable class="top-left" @move="(x, y) => { move(x, y); resize(-x, -y) }" />
    </div>
  </div>
</template>

<script>
import Dragable from './Dragable.vue';

export default {
  name: "Rect",
  components: {
      Dragable,
  },
  props: {
    id: String,
    selected: {
      type: Boolean,
      default: false,
    },
    left: Number,
    top: Number,
    width: Number,
    height: Number,
  },
  data() {
    return {
      style: {
        border: '1px solid #000',
      },
    };
  },
  methods: {
    onMousedown() {
      this.$emit('select', this.id);
    },
    move(x, y) {
      this.$emit('move', this.id, x, y);
    },
    resize(x, y) {
      this.$emit('resize', this.id, x, y);
    }
  },
  computed: {
    transpiledStyle() {
      return Object.assign(this.style, {
        left: `${this.left}px`,
        top: `${this.top}px`,
        width: `${this.width}px`,
        height: `${this.height}px`,
        position: 'absolute',
      });
    },
  }
};
</script>

<style scoped lang="scss">
.item.selected:after {
  content: "";
  width: calc(100% + 2px);
  height: calc(100% + 2px);
  border: 1px solid #f00;
  position: absolute;
  left: -2px;
  top: -2px;
}

.handles {
    div {
        width: 10px;
        height: 10px;
        border: 1px solid #f00;
        background-color: white;
        position: absolute;
        z-index: 1;
        &.top {
            top: -7px;
            left: calc(50% - 5px);
        }
        &.top-right {
            top: -7px;
            right: -7px;
        }
        &.right {
            right: -7px;
            top: calc(50% - 5px);
        }
        &.bottom-right {
            bottom: -7px;
            right: -7px;
        }
        &.bottom {
            bottom: -7px;
            left: calc(50% - 5px);
        }
        &.bottom-left {
            bottom: -7px;
            left: -7px;
        }
        &.left {
            left: -7px;
            top: calc(50% - 5px);
        }
        &.top-left {
            top: -7px;
            left: -7px;
        }
    }
}
</style>
