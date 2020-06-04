<template>
  <div>
    <div
      class="node"
      :style="{'margin-left': `${depth * 20}px`}"
      @click="nodeClicked"
    >
      <span
        v-if="hasChildren"
        class="type"
      >
        {{expanded ? '&#9660;' : '&#9658;'}}
      </span>
      <span v-else class="type">&#9671;</span>
      <span :style="getStyle(node)">
        {{ node.name }}
      </span>
    </div>
    <div v-if="expanded">
      <TreeBrowser
        v-for="child in node.children"
        :key="child.name"
        :node="child"
        :depth="depth + 1"
        @onClick="(node) => $emit('onClick', node)"
      />
    </div>
  </div>
</template>

<script>
import * as ColorHash from 'color-hash';

const colorHash = new ColorHash();

export default {
  name: 'TreeBrowser',
  props: {
    node: Object,
    depth: {
      type: Number,
      default: 0,
    },
  },
  data() {
    return {
      expanded: false,
    };
  },
  methods: {
    nodeClicked() {
      this.expanded = !this.expanded;
      if (!this.hasChildren) {
        this.$emit('onClick', this.node);
      }
    },
    getStyle(node) {
      const color = !node.children ? colorHash.hex(node.name.split('.')[1]) : '#f22';
      return { color };
    },
  },
  computed: {
    hasChildren() {
      return this.node.children;
    },
  },
};
</script>

<style lang="scss" scoped>
.node {
  text-align: left;
  font-size: 18px;
  cursor: pointer;
}

.type {
  margin-right: 10px;
}
</style>
