<template>
  <div class="card-devices">
    <div class="header">
      <h3>Number</h3>
    </div>
    <div class="node-body">
      <h5 class="subtitle">Number</h5>
      <input class="input-area" type="number" df-name />
    </div>
  </div>
</template>

<script>
export default {
  props: [],
  methods: {},
};
</script>

<style>
.input-area {
  width: 100%;
}

.subtitle {
  text-align: right;
}
</style>
