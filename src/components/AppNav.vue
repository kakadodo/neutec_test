<script setup>
import { ref, onMounted, computed } from "vue";
import Menu from "./Menu.vue";

const list = ref([]);
const isDrawerOpen = ref(false);
const focusItemKey = ref("");

const toggleDrawer = (isShow) => {
  isDrawerOpen.value = isShow;
};

const onUpdateFocusItem = (key) => {
  focusItemKey.value = key;
  localStorage.setItem("FOCUS_ITEM_KEY", key);
};

const flattenList = ref([]);
const getFlattenMenu = (menu) => {
  const flatMenu = [];
  const flatten = (menu) => {
    menu.forEach((item) => {
      flatMenu.push({ key: item.key, text: item.text });
      if (item.children) {
        flatten(item.children);
      }
    });
  };
  flatten(menu);
  return flatMenu;
};

onMounted(async () => {
  const data = await fetch(`${import.meta.env.BASE_URL}/api/menu.json`);
  list.value = await data.json();
  focusItemKey.value = localStorage.getItem("FOCUS_ITEM_KEY") ?? "";
  flattenList.value = getFlattenMenu(list.value);
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
        <select v-model="focusItemKey" class="select">
          <option value="" disabled>--</option>
          <option
            v-for="item in flattenList"
            :value="item.key"
            :key="item.key"
          >
            {{ item.text }}
          </option>
        </select>
        <Menu
          :list="list"
          :focusItemKey="focusItemKey"
          @updateFocusItem="onUpdateFocusItem"
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
.select {
  margin: 10px 20px;
}
</style>
