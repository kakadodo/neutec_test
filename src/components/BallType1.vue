<script setup>
defineProps({
  boxes: Array
})
</script>

<template>
  <div class="ball-wrapper">
    <template v-for="box in boxes" :key="box.id">
      <div :class="['ball', `ball-${box.id}`]" v-if="box.hasBall">0</div>
    </template>
  </div>
</template>

<style scoped lang="scss">
.ball-wrapper {
  animation: ballMoveRight 1.5s infinite;
}
@keyframes ballMoveRight {
  from {
    transform: translateX(0);
  }
  to {
    transform: translateX(66%);
  }
}
</style>
