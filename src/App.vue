<template>
  <el-config-provider namespace="ep">
    <DialogDemo />
  </el-config-provider>
</template>

<script setup>
  import DialogDemo from './components/DialogDemo.vue';
</script>

<style>
#app {
  text-align: center;
  color: var(--ep-text-color-primary);
}

.main-container {
  height: calc(100vh - var(--ep-menu-item-height) - 3px);
}
</style>
