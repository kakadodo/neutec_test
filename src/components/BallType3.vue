<script setup>
import { computed, onBeforeUnmount, onMounted, ref } from "vue";

const ballWrapperRef = ref();
const timer = ref(null);
const ballCount = ref(0);
const ballList = ref([]);

const createBall = () => {
  const id = ballCount.value;
  ballList.value.push({
    id,
    left: Math.random() * 100 - (15 / ballWrapperRef.value.offsetWidth * 100),
    top: Math.random() * 100 - (15 / ballWrapperRef.value.offsetHeight * 100),
  });
  setTimeout(() => {
    ballList.value[id].left = Math.random() * 100 - (15 / ballWrapperRef.value.offsetWidth * 100);
    ballList.value[id].top = Math.random() * 100 - (15 / ballWrapperRef.value.offsetHeight * 100);
  }, 300);
};
onMounted(() => {
  timer.value = setInterval(() => {
    if (ballCount.value >= 99) clearInterval(timer.value);
    createBall();
    ballCount.value++;
  }, 500);
});
onBeforeUnmount(() => {
  clearInterval(timer.value);
})
</script>

<template>
  <div ref="ballWrapperRef" class="ball-wrapper">
    <div
      class="ball"
      v-for="ball in ballList"
      :key="ball.id"
      :style="{left: `${ball.left}%`, top: `${ball.top}%`}"
    >
      0
    </div>
  </div>
</template>

<style scoped lang="scss">
.ball {
  pointer-events: none;
  transition: left .5s, top .5s;
}
</style>
