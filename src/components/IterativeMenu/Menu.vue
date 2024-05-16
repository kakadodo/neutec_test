<script setup>
import { computed } from "vue";

defineOptions({
  name: "MenuIterative",
});
const props = defineProps({
  list: {
    type: Array,
    required: true,
  },
  parentKey: {
    type: String,
    required: true,
  },
  focusItemKey: {
    type: String,
    required: true,
  },
});
const emit = defineEmits(["updateFocusItem"]);
const getChildItems = computed(() => {
  return props.list.filter((item) => item.parentKey === props.parentKey);
});
const hasChildren = (key) => {
  return props.list.some((item) => item.parentKey === key);
};
const onClick = (item) => {
  emit("updateFocusItem", item.key);
};
</script>

<template>
  <ul class="menu">
    <li
      class="menu-item"
      :class="{ active: focusItemKey === item.key }"
      v-for="item in getChildItems"
      :key="item.key"
      @click.stop="onClick(item)"
    >
      <div class="name">{{ item.text }}</div>
      <MenuIterative
        v-if="hasChildren(item.key)"
        :list="list"
        :focusItemKey="focusItemKey"
        :parentKey="item.key"
        :key="item.key"
        @updateFocusItem="(key)=> $emit('updateFocusItem', key)"
      />
    </li>
  </ul>
</template>

<style scoped>
.menu {
  &:has(.menu-item.active) {
    display: block;
  }
}
.menu-item {
  padding-left: 20px;
  padding-top: 10px;
  padding-bottom: 10px;
  cursor: pointer;

  .name {
    padding: 10px 0;

    ~ .menu .menu-item {
      padding-top: 0;
      padding-bottom: 0;
    }
  }

  .menu {
    display: none;
  }

  &.active {
    background-color: #666;
    > .name {
      color: yellow;
    }
    > .menu {
      display: block;
    }
  }

  &:has(.menu-item.active) {
    background-color: #666;
    > .name {
      color: yellow;
    }
  }
}
</style>
