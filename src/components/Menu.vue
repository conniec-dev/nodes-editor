<template>
  <h2 class="item-title">Menu</h2>
  <div
    class="item"
    v-for="item in menuItems"
    :key="item.itemName"
    @click="handleItemClick(item.id)"
  >
    <img :src="item.icon" class="icons" />
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
