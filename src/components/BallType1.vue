<script setup>
import { onBeforeUnmount, onMounted, ref } from "vue";
defineProps({
  boxes: Array,
});

const timer = ref(null);
const isBallMove = ref(false);
const moveBall = () => {
  timer.value = setTimeout(
    () => {
      isBallMove.value = !isBallMove.value;
      moveBall();
    },
    isBallMove.value ? 1500 : 10
  );
};
onMounted(() => {
  moveBall();
});
onBeforeUnmount(() => {
  clearTimeout(timer.value);
});
</script>

<template>
  <div :class="['ball-wrapper', { 'ball-move': isBallMove }]">
    <template v-for="box in boxes" :key="box.id">
      <div :class="['ball', `ball-${box.id}`]" v-if="box.hasBall">0</div>
    </template>
  </div>
</template>

<style scoped lang="scss">
.ball-wrapper.ball-move {
  .ball {
    transition: left 1.5s, top 1.5s;
    @for $i from 1 through 9 {
      @if $i < 4 {
        &.ball-#{$i} {
          left: calc(100% / 3 * $i - (100% / 3 / 2) + 66%);
        }
      } @else if $i < 7 {
        &.ball-#{$i} {
          left: calc((100% / 3 * $i) - (100% / 3 * 3) - (100% / 3 / 2) + 66%);
        }
      } @else {
        &.ball-#{$i} {
          left: calc((100% / 3 * $i) - (100% / 3 * 6) - (100% / 3 / 2) + 66%);
        }
      }
    }
  }
}
</style>
