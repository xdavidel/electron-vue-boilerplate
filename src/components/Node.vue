<template>
  <li>
    <div :class="{bold: isFolder}" @click="toggle" @dblclick="changeType">
      {{ model.name }}
      <span v-if="isFolder">[{{ open ? '-' : '+' }}]</span>
    </div>
    <ul v-show="open" v-if="isFolder">
      <node class="node" v-for="(child, index) in model.children" :key="index" :model="child">
      </node>

      <li class="add" @click="addChild">+</li>
    </ul>
  </li>
</template>

<script>
import Vue from "vue";

export default {
  props: {
    model: Object
  },
  data: function() {
    return {
      open: false
    };
  },
  beforeCreate: function() {
    this.$options.components.Node = require("./Node.vue").default;
  },
  computed: {
    isFolder: function() {
      return this.model.children && this.model.children.length;
    }
  },
  methods: {
    toggle: function() {
      if (this.isFolder) {
        this.open = !this.open;
      }
    },
    changeType: function() {
      if (!this.isFolder) {
        Vue.set(this.model, "children", []);
        this.addChild();
        this.open = true;
      }
    },
    addChild: function() {
      this.model.children.push({
        name: "new stuff"
      });
    }
  }
};
</script>
<style scoped>
.node {
  cursor: pointer;
}
.bold {
  font-weight: bold;
}
ul {
  padding-left: 1em;
  line-height: 1.5em;
  list-style-type: dot;
}
</style>
