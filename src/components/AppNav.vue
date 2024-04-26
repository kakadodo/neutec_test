<script setup>
import { ref, onMounted } from "vue";
import Menu from './Menu.vue';

const list = ref([]);
const isDrawerOpen = ref(true);
const focusItemKey = ref('');

const toggleDrawer = (isShow) => {
  isDrawerOpen.value = isShow;
};

const onUpdateFocusItem = (key) => {
  console.log(key);
  focusItemKey.value = key;
  localStorage.setItem('FOCUS_ITEM_KEY', key);
}

onMounted(async () => {
  const data = await fetch('/api/menu.json');
  list.value = await data.json();
  focusItemKey.value = localStorage.getItem('FOCUS_ITEM_KEY') ?? '';
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
        <Menu :list="list" :focusItemKey="focusItemKey" @updateFocusItem="onUpdateFocusItem" />
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
  position: fixed;
  right: 0;
  top: 0;
  width: 100%;
  height: 100vh;
  transform: translateX(100%);
  transition: transform 0.3s;
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
    padding: 20px;
    width: 50vw;
    height: 100%;
    background-color: rgba(0, 0, 0, 85%);
    color: #fff;
  }
}
</style>
