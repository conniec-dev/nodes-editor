<template>
  <div class="card-devices">
    <div class="header">
      <h3>{{ name }}</h3>
    </div>
    <div class="body">
      <h5 class="subtitle">Number</h5>
      <input type="number" df-name />
    </div>
  </div>
</template>

<script>
export default {
  props: ["name"],
  methods: {
    Clicked() {
      alert("hello-drawflow");
    },
  },
};
</script>

<style>
.subtitle {
  text-align: right;
}
</style>
