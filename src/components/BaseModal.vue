<template>
  <Transition name="modal-outer">
    <div v-show="modalState"
      class="absolute w-full h-screen bg-black bg-opacity-30 top-0 left-0 flex justify-center px-8"
    >
      <Transition name="modal-inner">
        <div v-if="modalState" class="p-4 bg-white self-start text-black mt-32 max-w-screen-md">
          <slot />
          <button @click="$emit('close-modal')"
            class="text-white bg-weather-primary drop-shadow-lg shadow-black px-6 mt-8 py-2 duration-150 hover:opacity-95"
          >
            Close
          </button>
        </div>
      </Transition>
    </div>
  </Transition>
</template>

<script lang="ts" setup>
defineEmits(['close-modal']);
defineProps({
  modalState: {
    type: Boolean,
    default: false,
  },
});
</script>

<style scoped>
.modal-outer-enter-active,
.modal-outer-leave-active {
  transition: opacity 0.3s cubic-bezier(0.52, 0.02, 0.19, 1.02);
}
.modal-outer-enter-from,
.modal-outer-leave-to {
  opacity: 0;
}
.modal-inner-enter-active {
  transition: all 0.3s cubic-bezier(0.52, 0.02, 0.19, 1.02) 0.15s;
}
.modal-inner-leave-active {
  transition: all 0.3s cubic-bezier(0.52, 0.02, 0.19, 1.02);
}
.modal-inner-enter-from {
  opacity: 0;
  transform: scale(0.8);
}
.modal-inner-leave-to {
  transform: scale(0.8);
}
</style>
