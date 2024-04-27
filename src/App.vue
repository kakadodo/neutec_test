<script setup>
import AppNav from './components/AppNav.vue';
import Boxes from './components/Boxes.vue';
</script>

<template>
  <div class="wrapper">
    <AppNav />
    <div class="main">
      <Boxes />
    </div>
  </div>
</template>

<style scoped>
.wrapper {
  position: relative;
  margin: 0 auto;
  max-width: 390px;
  max-height: 844px;
  height: 100vh;
  display: flex;
  flex-direction: column;
  overflow: hidden;
}
.main {
  flex: 1;
  display: flex;
  align-items: center;
  background-color: #ccc;
}
</style>
