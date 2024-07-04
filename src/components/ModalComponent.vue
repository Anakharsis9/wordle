<script setup lang="ts">
import { ref } from "vue";
import { onClickOutside } from "@vueuse/core";

const props = defineProps<{
  isOpen: boolean;
}>();

const emit = defineEmits<{
  "modal-close": [];
}>();

const target = ref(null);
onClickOutside(target, () => emit("modal-close"));
</script>

<template>
  <Transition name="slide-up">
    <div v-if="isOpen" class="modal-mask">
      <div class="modal-wrapper">
        <div class="modal-container" ref="target">
          <div class="modal-header">
            <div class="close-icon__wrapper">
              <img
                src="/src/assets/icons/close-icon.svg"
                alt="close"
                class="close-icon"
                @click.stop="emit('modal-close')"
              />
            </div>
          </div>
          <div class="modal-body">
            <slot name="content"> default content </slot>
          </div>
        </div>
      </div>
    </div>
  </Transition>
</template>

<style scoped>
.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
}
.modal-container {
  width: 300px;
  margin: 150px auto;
  padding: 20px 30px;
  background-color: var(--background);
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
}
.close-icon__wrapper {
  display: flex;
  align-items: center;
  justify-content: flex-end;
}
.close-icon {
  width: 30px;
  height: 30px;
  cursor: pointer;
}
.slide-up-enter-active {
  transition: all 0.3s ease-out;
}

.slide-up-leave-active {
  transition: all 0.3s ease-out;
}

.slide-up-enter-from,
.slide-up-leave-to {
  transform: translateY(20px);
  opacity: 0;
}
</style>
