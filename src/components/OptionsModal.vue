<template>
  <transition name="modal-animation"></transition>
  <div v-show="modalActive" class="modal">
    <transition name="modal-animation-inner">
      <div v-show="modalActive" class="modal-inner">
        <slot />
      </div>
    </transition>
  </div>
</template>

<script>
export default {
  props: ["modalActive"],
  emits: ["close"],
  setup(props, { emit }) {
    const close = () => {
      emit("close");
    };

    return { close };
  },
};
</script>

<style>
.modal {
  font-size: 15px;
}

/* .modal {
  align-self: flex-end;
  width: 100%;
  font-size: 15px;
  padding-left: 10px;
} */
</style>
