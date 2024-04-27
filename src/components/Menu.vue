<script setup>
import MenuItem from "./MenuItem.vue";
defineProps({
  list: Array,
  focusItemKey: String,
});

const emit = defineEmits(["updateFocusItem"]);

const onClickItem = (key) => {
  emit("updateFocusItem", key);
};
</script>

<template>
  <ul class="menu">
    <MenuItem
      v-for="item in list"
      :key="item.key"
      :item="item"
      :focusItemKey="focusItemKey"
      @clickItem="onClickItem"
      @updateFocusItem="onClickItem"
    />
  </ul>
</template>

<style scoped>
.menu {
  
  &:has(.menu-item.active) {
    display: block;
  }
}
</style>
