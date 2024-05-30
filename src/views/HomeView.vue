<script setup lang="ts">
import { onMounted, onUnmounted, ref } from "vue";
import BoardTile from "../components/BoardTile.vue";
import KeyButton from "../components/KeyButton.vue";

const keyWord = ref("gummy");
const currentRowIndex = ref(0);
const userWords = ref([]);

function onKeyPressed(c: string) {}

const keyboardListener = (e: KeyboardEvent) => {
  // console.log(e.code.includes("Key"));
};

onMounted(() => {
  window.addEventListener("keydown", keyboardListener);
});

onUnmounted(() => {
  window.removeEventListener("keydown", keyboardListener);
});
</script>

<template>
  <main class="game__container">
    <div class="game__board">
      <div v-for="n in 6" :key="n" class="board__row">
        <board-tile v-for="m in 5" :key="m" />
      </div>
    </div>
    <div class="keyboard__container">
      <div class="keyboard__row">
        <key-button
          v-for="c in 'qwertyuiop'"
          :key="c"
          :char="c"
          @click="onKeyPressed(c)"
        />
      </div>
      <div class="keyboard__row">
        <key-button
          v-for="c in 'asdfghjkl'"
          :key="c"
          :char="c"
          @click="onKeyPressed(c)"
        />
      </div>
      <div class="keyboard__row">
        <key-button char="enter" isWide @click="onKeyPressed('enter')" />
        <key-button
          v-for="c in 'zxcvbnm'"
          :key="c"
          :char="c"
          @click="onKeyPressed(c)"
        />
        <key-button isWide isBackspace @click="onKeyPressed('backspace')" />
      </div>
    </div>
  </main>
</template>

<style scoped>
.game__container {
  margin-top: 65px;
  padding-top: 30px;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.game__board {
  width: 330px;
  height: 400px;
  display: grid;
  grid-template-rows: repeat(6, 1fr);
  gap: 5px;
  padding: 10px;
  margin-bottom: 24px;
}
.board__row {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 5px;
}

.keyboard__container {
  height: 200px;
  padding: 0 8px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 8px;
}
.keyboard__row {
  display: flex;
  gap: 6px;
  width: 100%;
  align-items: center;
  justify-content: center;
}
</style>
