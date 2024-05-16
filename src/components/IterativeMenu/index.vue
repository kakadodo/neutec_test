<script setup>
import { computed, ref } from "vue";
import Menu from "./Menu.vue";
const props = defineProps({
  originList: {
    type: Array,
    required: true,
  },
  focusItemKey: {
    type: String,
    required: true,
  },
  onUpdateFocusItem: {
    type: Function,
    required: true,
  },
});

// 使用迭代來做
const flattenList = computed(() => flattenMenuWithParent(props.originList));
const flattenMenuWithParent = (menu) => {
  const stack = [...menu];
  const flatMenu = [];

  while (stack.length) {
    const item = stack.shift();
    const displayName = item.parentName
      ? `${item.parentName}->${item.text}`
      : item.text;
    const parentKey = item.parentKey ?? "root";
    const newItem = { ...item, parentKey, displayName };
    flatMenu.push(newItem);
    if (item.children) {
      stack.unshift(
        ...item.children.map((child) => ({
          ...child,
          parentKey: item.key,
          parentName: displayName,
        }))
      );
    }
  }
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
    <option
      v-for="item in flattenList"
      :value="item.key"
      :key="item.key"
    >
      {{ item.displayName }}
    </option>
  </select>
  <Menu
    :list="flattenList"
    :focusItemKey="focusItemKey"
    parentKey="root"
    key="root"
    @updateFocusItem="onUpdateFocusItem"
  />
</template>

<style></style>
