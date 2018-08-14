<template>
  <li>
    <div v-show="!edit" :class="{bold: isFolder}" @click="toggle" @dblclick="editMode" @click.right="changeType">
      {{ model.name }}
      <span v-if="isFolder">[{{ open ? '-' : '+' }}]</span>
    </div>
    <form @submit.prevent="endEdit">

    <input type="text" v-show="edit" v-model="model.name" @blur="endEdit">
    </form>
    <ul v-show="open" v-if="isFolder">
      <node class="node" v-for="(child, index) in model.children" :key="index" :parent="model" :index="index" :model="child" >
      </node>

      <li class="add" @click="addChild">+</li>
    </ul>
  </li>
</template>

<script>
import Vue from "vue";

export default {
  props: {
    model: Object,
    parent: Object,
    index: Number
  },
  data: function() {
    return {
      open: false,
      edit: false
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
    },
    deleteNode: function() {
      if (this.isFolder) {
        this.model.children.splice(0, this.model.children.length);
      }
      if (this.parent) {
        this.parent.children.splice(this.index, 1);
      }
    },
    editMode: function() {
      this.edit = true;
    },
    endEdit: function() {
      this.edit = false;

      if (this.model.name.length <= 0) {
        this.deleteNode();
      }
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
