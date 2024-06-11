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
  <div v-if="isOpen" class="modal-mask">
    <div class="modal-wrapper">
      <div class="modal-container">
        <div class="modal-header">
          <div class="close-icon" @click.stop="emit('modal-close')">
            <img src="/src/assets/icons/close-icon.svg" alt="close" />
          </div>
        </div>
        <div class="modal-body">
          <slot name="content"> default content </slot>
        </div>
      </div>
    </div>
  </div>
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
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
}
.close-icon {
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
}
</style>
