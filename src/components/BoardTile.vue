<script setup lang="ts">
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
</script>

<template>
  <div class="board__tile">{{ letter }}</div>
</template>

<style scoped>
.board__tile {
  border: 2px solid var(--main-gray);

  display: inline-flex;
  justify-content: center;
  align-items: center;
  font-size: 2rem;
  text-transform: uppercase;
  line-height: 1;
  font-weight: bold;
  vertical-align: middle;
}
</style>
