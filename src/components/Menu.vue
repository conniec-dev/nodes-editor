<template>
  <h2>Menu</h2>
  <div
    class="item"
    v-for="item in menuItems"
    :key="item.itemName"
    @click="handleItemClick(item.id)"
  >
    <img src="../icons/block.svg" width="20" height="20" />
    {{ item.itemName }}
  </div>
</template>

<script>
export default {
  props: ["menuItems"],
  emits: ["itemClick"],
  components: {},
  data() {
    return {};
  },
  methods: {
    handleItemClick(itemId) {
      this.$emit("itemClick", { itemId });
    },
  },
};
</script>
