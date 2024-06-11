<script setup lang="ts">
import { computed } from "vue";

const props = withDefaults(
  defineProps<{
    char?: string;
    isWide?: boolean;
    isBackspace?: boolean;
    color?: "transparent" | "green" | "yellow" | "gray";
  }>(),
  {
    isWide: false,
    isBackspace: false
  }
);

const colorStyle = computed(() => ({
  backgroundColor: `var(--${props.color})`
}));
</script>

<template>
  <button
    type="button"
    class="key__button"
    :style="props.color && colorStyle"
    :class="{ 'key__button-wide': isWide }"
  >
    {{ char }}
    <img
      v-if="isBackspace"
      src="/src/assets/icons/backspace-icon.svg"
      alt="backspace"
    />
  </button>
</template>

<style scoped>
.key__button {
  outline: none;
  font-size: 1.25em;
  font-weight: bold;
  border: 0;
  padding: 0;
  height: 58px;
  width: 43px;
  border-radius: 4px;
  cursor: pointer;
  background-color: var(--light-gray);
  text-transform: uppercase;
  transition: background-color 0.1s linear;
}
.key__button-wide {
  flex: 0.9;
  font-size: 12px;
}
</style>
