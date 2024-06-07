<script setup lang="ts">
import { computed } from "vue";

const props = withDefaults(
  defineProps<{
    letter?: string;
    color?: "transparent" | "green" | "yellow" | "gray";
  }>(),
  {
    letter: "",
    color: "transparent"
  }
);
const colorStyle = computed(() => ({
  backgroundColor: `var(--${props.color})`,
  border: `2px solid var(--${
    props.color === "transparent" ? "gray" : props.color
  })`
}));
</script>

<template>
  <div class="board__tile" :style="colorStyle">{{ letter }}</div>
</template>

<style scoped>
.board__tile {
  border: 2px solid var(--gray);

  display: inline-flex;
  justify-content: center;
  align-items: center;
  font-size: 2rem;
  text-transform: uppercase;
  line-height: 1;
  font-weight: bold;
  vertical-align: middle;
  background-color: var(--g);
}
</style>
