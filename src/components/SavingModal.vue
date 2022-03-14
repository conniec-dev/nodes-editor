<template>
  <transition name="save-modal-animation">
    <div v-show="saveModalActive" id="save-modal-container">
      <div class="save-modal">
        <transition name="save-modal-animation-inner">
          <div v-show="saveModalActive" class="save-modal-inner">
            <img
              @click="closeSaveModal"
              :src="require('../icons/close3.svg')"
              class="close-icon"
            />
            <!-- Modal Content  -->
            <slot />
          </div>
        </transition>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  props: ["saveModalActive"],
  emits: ["closeSaveModal"],
  setup(props, { emit }) {
    const close = () => {
      emit("close");
    };

    return { close };
  },
};
</script>

<style>
.save-modal-inner {
  border: 1px solid orchid;
  display: flex;
  flex-direction: column;
  flex-wrap: wrap;
  background-color: white;
  height: 30%;
  width: 50%;
  padding: 20px;
}

.close-icon {
  height: 30px;
  width: 30px;
  align-self: flex-end;
  cursor: pointer;
}
</style>