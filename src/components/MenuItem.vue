<script setup>
import Menu from "./Menu.vue";
defineProps({
  item: Object,
  focusItemKey: String,
});

const emit = defineEmits(["clickItem", "updateFocusItem"]);

const onClick = (item) => {
  emit("clickItem", item.key);
};

const onUpdateFocusItem = (key) => {
  emit("updateFocusItem", key);
}
</script>

<template>
  <li :class="['menu-item', {active: focusItemKey === item.key }]" @click.stop="onClick(item)">
    <div class="name">{{ item.text }}</div>
    <Menu
      v-if="item.children"
      :key="`child-${item.key}`"
      :list="item.children"
      :focusItemKey="focusItemKey"
      @updateFocusItem="onUpdateFocusItem"
    />
  </li>
</template>

<style scoped lang="scss">
.menu-item {
  margin: 20px 0;

  .menu {
    display: none;
  }

  &.active {
    > .name {
      color: yellow;
    }
    > .menu {
      display: block;
    }
  }

  &:has(.menu-item.active) {
    > .name {
      color: yellow;
    }
  }
}
</style>
