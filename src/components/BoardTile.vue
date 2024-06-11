<script setup lang="ts">
import { computed, ref, watch } from "vue";

const props = withDefaults(
  defineProps<{
    letter?: string;
    color?: "transparent" | "green" | "yellow" | "gray";
    delay?: number;
    playWinAnimation?: boolean;
  }>(),
  {
    letter: "",
    color: "transparent",
    delay: 0
  }
);
const colorStyle = computed(() => ({
  backgroundColor: `var(--${props.color})`,
  border: `2px solid var(--${
    props.color === "transparent" ? "gray" : props.color
  })`
}));

const playRotate = ref(false);
const changeColor = ref(false);

watch(
  () => props.color,
  () => {
    setTimeout(() => {
      playRotate.value = true;
    }, props.delay);

    setTimeout(() => {
      changeColor.value = true;
      playRotate.value = false;
    }, props.delay + 500);
  }
);

const playJump = ref(false);
watch(
  () => props.playWinAnimation,
  () => {
    setTimeout(() => {
      playJump.value = true;
    }, props.delay + 1000);

    setTimeout(() => {
      playJump.value = false;
    }, props.delay + 1300);
  }
);
</script>

<template>
  <div
    class="board__tile"
    :style="[changeColor ? colorStyle : '']"
    :class="{
      'letter-entered': letter,
      rotate: playRotate,
      jump: playJump
    }"
  >
    {{ letter }}
  </div>
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
  transition: transform 0.3s linear;
  transform: rotateX(0deg);
}

.letter-entered {
  animation: scale 0.1s linear 0ms;
}

.rotate {
  transform: rotateX(90deg);
}

.jump {
  transform: translateY(-25%);
}

@keyframes scale {
  0% {
    transform: scale(1);
  }
  50% {
    transform: scale(0.9);
  }
  100% {
    transform: scale(1);
  }
}
</style>
