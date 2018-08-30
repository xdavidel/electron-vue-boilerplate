<template>
  <div class="tree">
    <h1>Tree View</h1>
    <div>
      <p>(You can double click on an item to turn it into a folder.)</p>
    <ul>
        <node class="node" :model="treeData"></node>    
    </ul>
    </div>
  </div>
</template>

<script>
import Node from "./Node.vue";

export default {
  data() {
    return {
      treeData: {
        name: "My Tree",
        children: [
          { name: "hello" },
          { name: "wat" },
          {
            name: "child folder",
            children: [
              {
                name: "child folder",
                children: [{ name: "hello" }, { name: "wat" }]
              },
              { name: "hello" },
              { name: "wat" },
              {
                name: "child folder",
                children: [{ name: "hello" }, { name: "wat" }]
              }
            ]
          }
        ]
      }
    };
  },
  components: {
    Node
  }
};
</script>

<style scoped>
.tree {
  text-align: center;
  margin: 0 auto;
}
.node {
  cursor: pointer;
}
.bold {
  font-weight: bold;
}
</style>
