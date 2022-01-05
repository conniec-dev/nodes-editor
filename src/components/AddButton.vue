<template>
  <div id="basic-event">
    <button @click="counter += 1">Add 1</button>
    <p>The button above has been clicked {{ counter }} times.</p>
  </div>
</template>

<script>
export default {
  name: "AddButton",
  data() {
    return {
      counter: 0,
    };
  },
  methods: {},
};
</script>
