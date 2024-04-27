<script setup>
import { ref } from "vue";
import boxesData from "../demo/boxes.json";
import BallType1 from "./BallType1.vue";
import BallType2 from "./BallType2.vue";
defineProps({
  ballType: Number
})
const boxes = ref(boxesData);
</script>

<template>
  <div class="box-wrapper">
    <div
      :class="['box', { isFlashing: box.isFlashing }]"
      v-for="box in boxes"
      :key="box.id"
    ></div>
    <BallType1 v-if="ballType === 1" :boxes="boxes" />
    <BallType2 v-if="ballType === 2" />
  </div>
</template>

<style scoped lang="scss">
.box-wrapper {
  position: relative;
  width: 100%;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 5px;
}
.box {
  position: relative;
  width: calc(33.333% - 6.666px);
  height: 100px;
  border: black solid 2px;
  background: radial-gradient(
    circle,
    rgba(113, 81, 95, 1) 81%,
    rgba(0, 0, 0, 1) 100%
  );
  &.isFlashing::before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    display: block;
    width: 100%;
    height: 100%;
    background-color: rgba(255, 255, 255, 0.5);
    animation: boxFlashing 0.5s infinite;
  }
}
@keyframes boxFlashing {
  from {
    opacity: 1;
  }
  to {
    opacity: 0.6;
  }
}
.ball-wrapper {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
:deep(.ball) {
  position: absolute;
  width: 30px;
  height: 30px;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 100px;
  background-color: #a5f12b;
  @for $i from 1 through 9 {
    @if $i < 4 {
      &.ball-#{$i} {
        left: calc(100% / 3 * $i - (100% / 3 / 2));
        top: calc(100% / 3 - (100% / 3 / 2) - 15px);
      }
    } @else if $i < 7 {
      &.ball-#{$i} {
        left: calc((100% / 3 * $i) - (100% / 3 * 3) - (100% / 3 / 2));
        top: calc(100% / 3 * 2 - (100% / 3 / 2) - 15px);
      }
    } @else {
      &.ball-#{$i} {
        left: calc((100% / 3 * $i) - (100% / 3 * 6) - (100% / 3 / 2));
        top: calc(100% / 3 * 3 - (100% / 3 / 2) - 15px);
      }
    }
  }
}
</style>
