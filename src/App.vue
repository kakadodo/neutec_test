<script setup>
import { ref } from "vue";
import AppNav from "./components/AppNav.vue";
import Boxes from "./components/Boxes.vue";

const ballType = ref(1);
const switchBallType = (type) => {
  ballType.value = type;
};
</script>

<template>
  <div class="wrapper">
    <AppNav />
    <div class="main">
      <div class="switch">
        <button
          :class="[{ active: ballType === 1 }]"
          @click="switchBallType(1)"
        >
          type1
        </button>
        <button
          :class="[{ active: ballType === 2 }]"
          @click="switchBallType(2)"
        >
          type2
        </button>
        <button
          :class="[{ active: ballType === 3 }]"
          @click="switchBallType(3)"
        >
          type3
        </button>
      </div>
      <Boxes :ballType="ballType" />
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
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-color: #ccc;
}
.switch {
  display: flex;
  gap: 10px;
  margin-bottom: 10px;
  
  button {
    padding: 4px 8px;
    border: 1px solid #000;
    background-color: #fff;

    &.active {
      background-color: yellow;
    }
  }
}
</style>
