<template>
  <div>
    <h1>{{ msg }}</h1>
    <clock></clock>
  </div>
</template>

<script>
import Clock from "./components/Clock.vue";

export default {
  data() {
    return {
      msg: "Welcome to Electron & Vue.js World!"
    };
  },
  components: {
    Clock
  }
};
</script>

<style scoped>
.container {
  padding-top: 70px;
}
</style>
