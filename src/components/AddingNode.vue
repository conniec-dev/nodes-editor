<template>
  <div class="card-devices">
    <div class="header">
      <h3>{{ name }}</h3>
    </div>
    <div class="body">
      <output></output>
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
