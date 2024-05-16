<script setup>
defineOptions({
  name: "MenuRecursive",
});
defineProps({
  list: Array,
  focusItemKey: String,
});

const emit = defineEmits(["updateFocusItem"]);

const onClick = (item) => {
  emit("updateFocusItem", item.key);
};
</script>

<template>
  <ul class="menu">
    <li
      :class="['menu-item', { active: focusItemKey === item.key }]"
      v-for="item in list"
      :key="item.key"
      @click.stop="onClick(item)"
    >
      <div class="name">{{ item.text }}</div>
      <MenuRecursive
        v-if="item.children"
        :key="`child-${item.key}`"
        :list="item.children"
        :focusItemKey="focusItemKey"
        @updateFocusItem="(key)=> $emit('updateFocusItem', key)"
      />
    </li>
  </ul>
</template>

<style scoped lang="scss">
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
