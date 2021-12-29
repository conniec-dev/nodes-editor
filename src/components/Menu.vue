<template>
  <h2>Menu</h2>
  <div class="item" v-for="item in menuItems" :key="item.itemName">
    {{ item.itemName }}
  </div>
</template>

<script>
export default {
  data() {
    return {
      menuItems: [
        { icon: " ", itemName: "Code Block", id: "1" },
        { icon: ".", itemName: "Variable", id: "2" },
        { icon: ".", itemName: "Arithmetic Operations", id: "3" },
        { icon: ".", itemName: "Conditionals", id: "4" },
        { icon: ".", itemName: "For Loop", id: "5" },
      ],
    };
  },
};
</script>
