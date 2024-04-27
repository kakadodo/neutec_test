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
};
</script>

<template>
  <li
    :class="['menu-item', { active: focusItemKey === item.key }]"
    @click.stop="onClick(item)"
  >
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
  padding-left: 20px;
  padding-top: 10px;
  padding-bottom: 10px;

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
