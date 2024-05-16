<script setup>
import { ref, onMounted } from "vue";
import RecursiveMenu from "./RecursiveMenu/index.vue";
import IterativeMenu from "./IterativeMenu/index.vue";

/** 原始數據 */
const list = ref([]);

/** 側邊選單開關 */
const isDrawerOpen = ref(false);
const toggleDrawer = (isShow) => {
  isDrawerOpen.value = isShow;
};

/** 側邊選單焦點項目 */
const focusItemKey = ref("");
const onUpdateFocusItem = (key) => {
  focusItemKey.value = key;
  localStorage.setItem("FOCUS_ITEM_KEY", key);
};

/** 菜單資料處理類型 */
const menuDataType = ref(1);
const switchMenuDataType = (type) => {
  menuDataType.value = type;
};

onMounted(async () => {
  const data = await fetch(`${import.meta.env.BASE_URL}/api/menu.json`);
  list.value = await data.json();
  focusItemKey.value = localStorage.getItem("FOCUS_ITEM_KEY") ?? "";
});
</script>

<template>
  <nav class="nav">
    <button class="nav-toggler" @click="toggleDrawer(true)">
      <svg
        xmlns="http://www.w3.org/2000/svg"
        height="24"
        viewBox="0 -960 960 960"
        width="24"
      >
        <path
          d="M120-240v-80h720v80H120Zm0-200v-80h720v80H120Zm0-200v-80h720v80H120Z"
        />
      </svg>
    </button>
    <div :class="['drawer', { show: isDrawerOpen }]">
      <div class="drawer-backdrop" @click="toggleDrawer(false)"></div>
      <div class="drawer-container">
        <div class="switch">
          <button
            :class="[{ active: menuDataType === 1 }]"
            @click="switchMenuDataType(1)"
          >
            遞迴
          </button>
          <button
            :class="[{ active: menuDataType === 2 }]"
            @click="switchMenuDataType(2)"
          >
            跌代
          </button>
        </div>
        <RecursiveMenu
          v-if="menuDataType === 1 && list.length"
          :originList="list"
          :focusItemKey="focusItemKey"
          :onUpdateFocusItem="onUpdateFocusItem"
        />
        <IterativeMenu
          v-if="menuDataType === 2 && list.length"
          :originList="list"
          :focusItemKey="focusItemKey"
          :onUpdateFocusItem="onUpdateFocusItem"
        />
      </div>
    </div>
  </nav>
</template>

<style scoped lang="scss">
.nav {
  display: flex;
  padding: 4px;
  height: 44px;
  background-color: #fff;
  &-toggler {
    margin-left: auto;
  }
}
.drawer {
  position: absolute;
  right: 0;
  top: 0;
  width: 100%;
  height: 100%;
  transform: translateX(100%);
  transition: transform 0.3s;
  z-index: 1;
  &.show {
    transform: translateX(0);
  }

  &-backdrop {
    position: absolute;
    width: 100%;
    height: 100%;
  }

  &-container {
    position: absolute;
    right: 0;
    top: 0;
    width: 50%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.9);
    color: #fff;
    overflow-y: auto;
  }
}
:deep(.select) {
  margin: 10px 20px;
  width: calc(100% - 40px);
}
</style>
