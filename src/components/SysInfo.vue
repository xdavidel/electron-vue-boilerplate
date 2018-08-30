<template>
  <div class="sysinfo">
    <h1>System Information</h1>

    
    <h2 class="group-header">App Version Data</h2>
    <ul class="list-group">
        <li class="list-item">Node: {{process.versions.node}}</li>
        <li class="list-item">Chrome: {{process.versions.chrome}}</li>
        <li class="list-item">Electron: {{process.versions.electron}}</li>
    </ul>
    <h2 class="group-header">System Specs</h2>
    <ul class="list-group">
        <li class="list-item">System Architecture: {{process.arch}}</li>
        <li class="list-item">Processor Identifier: {{process.env.PROCESSOR_IDENTIFIER}}</li>
        <li class="list-item">System OS: {{process.env.OS}}</li>
    </ul>
    <h2 class="group-header">System Memory</h2>
    <ul class="list-group">
        <li class="list-item">Total: {{process.getSystemMemoryInfo().total}}</li>
        <li class="list-item">Free: {{process.getSystemMemoryInfo().free}}</li>
        <li class="list-item">Swap Total: {{process.getSystemMemoryInfo().swapTotal}}</li>
        <li class="list-item">Swap Free: {{process.getSystemMemoryInfo().swapFree}}</li>
    </ul>
    <h2 class="group-header">Computer Info</h2>
    <ul class="list-group">
        <li class="list-item">User Domain: {{process.env.USERDOMAIN}}</li>
        <li class="list-item">Username: {{process.env.USERNAME}}</li>
        <li class="list-item">Home Folder: {{process.env.HOME}}</li>
        <li class="list-item">System Drive: {{process.env.SYSTEMDRIVE}}</li>
        <li class="list-item">System Root: {{process.env.SYSTEMROOT}}</li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return { process: process };
  },
  methods: {}
};
</script>

<style scoped>
.sysinfo {
  text-align: center;
  margin: 0 auto;
}

.page-header {
  color: rgb(26, 163, 115);
  animation-name: fade-in;
  animation-duration: 2s;
}

.group-header {
  color: #0073ff;
  display: inline-block;
  margin-top: 8px;
  margin-bottom: 5px;
  transition-property: transform;
  transition-duration: 1s;
}

.group-header:hover {
  transform: rotateY(360deg);
}

@keyframes fade-in {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}

.list-group {
  border: 2px solid rgb(57, 55, 55);
  padding: 3px;
  margin: 2px;
}

.list-item {
  list-style-type: none;
  padding: 4px;
}

.list-item:hover {
  background-color: #6393b1;
}
</style>
