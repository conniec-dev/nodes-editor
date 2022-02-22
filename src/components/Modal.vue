<template>
  <transition name="list-modal-animation">
    <div v-show="listModalActive" id="list-modal-container">
      <div class="list-modal">
        <transition name="list-modal-animation-inner">
          <div v-show="listModalActive" class="list-modal-inner">
            <img
              @click="close"
              :src="require('../icons/close.svg')"
              class="close-icon"
            />
            <!-- Modal Content  -->
            <slot />
            <button @click="close" type="button">Close</button>
          </div>
        </transition>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  props: ["listModalActive"],
  setup(props, { emit }) {
    const close = () => {
      emit("close");
    };

    return { close };
  },
};
</script>

<style>
.list-modal-inner {
  border: 1px solid orchid;
  display: flex;
  flex-direction: column;
  flex-wrap: wrap;
  background-color: white;
  height: 70%;
  width: 100%;
  padding: 20px;
}

.close-icon {
  height: 30px;
  width: 30px;
  align-self: flex-end;
}
</style>
