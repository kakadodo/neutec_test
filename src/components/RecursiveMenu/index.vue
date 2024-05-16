<script setup>
import { computed } from "vue";
import Menu from "./Menu.vue";
const props = defineProps({
  originList: {
    type: Array,
    required: true
  },
  focusItemKey: {
    type: String,
    required: true,
  },
  onUpdateFocusItem: {
    type: Function,
    required: true,
  }
});

// 使用遞迴來做
const flattenList = computed(() => getFlattenMenu(props.originList));
const getFlattenMenu = (menu) => {
  const flatMenu = [];
  const flatten = (menu, parentText) => {
    menu.forEach((item) => {
      let collection = parentText ? [...parentText, item.text] : [item.text];
      flatMenu.push({ key: item.key, text: collection.join("->") });
      if (item.children) {
        flatten(item.children, collection);
      }
    });
  };
  flatten(menu);
  return flatMenu;
};

</script>
<template>
  <select
    :value="focusItemKey"
    class="select"
    @change="onUpdateFocusItem($event.target.value)"
  >
    <option value="" disabled>--</option>
    <option v-for="item in flattenList" :value="item.key" :key="item.key">
      {{ item.text }}
    </option>
  </select>
  <Menu
    :list="originList"
    :focusItemKey="focusItemKey"
    @updateFocusItem="onUpdateFocusItem"
  />
</template>
<style></style>
