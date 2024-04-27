<script setup>
import { ref } from "vue";

const ballWrapperRef = ref();
const showBallPosition = [0, 2, 6, 8];
const ballMovePath = ref("");
const isTransition = ref(false);
const triggerTransitionCount = ref(0);

const getClickPosition = (event) => {
  if (isTransition.value) return;
  const moveOffsetXPercent =
    (event.offsetX / ballWrapperRef.value.offsetWidth) * 100;
  const moveOffsetYPercent =
    (event.offsetY / ballWrapperRef.value.offsetHeight) * 100;
  const ballSelfOffsetXPercent = (15 / ballWrapperRef.value.offsetWidth) * 100;
  const ballSelfOffsetYPercent = (15 / ballWrapperRef.value.offsetHeight) * 100;
  ballMovePath.value = `left: ${
    moveOffsetXPercent - ballSelfOffsetXPercent
  }%; top: ${moveOffsetYPercent - ballSelfOffsetYPercent}%`;
  isTransition.value = true;
};

const updateTransitionStatus = (event) => {
  triggerTransitionCount.value++;
  if (triggerTransitionCount.value >= showBallPosition.values.length * 2) {
    isTransition.value = false;
    triggerTransitionCount.value = 0;
  }
};
</script>

<template>
  <div ref="ballWrapperRef" class="ball-wrapper" @click="getClickPosition">
    <template v-for="(num, index) in 9" :key="index">
      <div
        @transitionend="updateTransitionStatus"
        :class="['ball', `ball-${num}`]"
        v-if="showBallPosition.includes(index)"
        :style="ballMovePath"
      >
        0
      </div>
    </template>
  </div>
</template>

<style scoped lang="scss">
.ball {
  pointer-events: none;
}
.ball-1 {
  transition: left 0.5s, top 0.5s;
}
.ball-3 {
  transition: left 0.5s 0.1s, top 0.5s 0.1s;
}
.ball-7 {
  transition: left 0.5s 0.2s, top 0.5s 0.2s;
}
.ball-9 {
  transition: left 0.5s 0.3s, top 0.5s 0.3s;
}
</style>
