<script setup lang="ts">
import { onMounted, onUnmounted, ref } from "vue";
import { Tile } from "../types";
import wordsData from "../words.json";
import BoardTile from "../components/BoardTile.vue";
import KeyButton from "../components/KeyButton.vue";

const keyWord = "melon";

let gameOver = false;

const createTile = (): Tile => ({
  letter: "",
  color: "transparent"
});
const createRow = (): Tile[] => Array.from({ length: 5 }, createTile);
const tiles = ref<Tile[][]>(Array.from({ length: 6 }, createRow));

const userWords = ref<string[]>([]);
const currentRowIndex = ref(1);
const currentTileIndex = ref(1);
const currentWord = ref("");

function onKeyPressed(c: string) {
  if (gameOver) return;
  if (currentRowIndex.value >= 7) return;
  if (currentTileIndex.value >= 6) return;
  currentWord.value += c;
  tiles.value[currentRowIndex.value - 1][currentTileIndex.value - 1].letter = c;

  currentTileIndex.value += 1;
}

function checkWord() {
  if (gameOver) return;
  if (currentTileIndex.value != 6) return;

  const isValidWord = wordsData.includes(currentWord.value);
  if (!isValidWord) {
    alert("Not in word list");
    return;
  }

  const keyWordDict: { [key: string]: number } = {};
  for (let char of keyWord) {
    keyWordDict[char] = (keyWordDict[char] || 0) + 1;
  }

  for (let i = 0; i < 5; i++) {
    const isExist = keyWordDict[currentWord.value[i]] > 0;
    if (isExist) {
      keyWordDict[currentWord.value[i]] -= 1;
    }
    tiles.value[currentRowIndex.value - 1][i].color =
      currentWord.value[i] === keyWord[i]
        ? "green"
        : isExist
        ? "yellow"
        : "gray";
  }
  if (currentWord.value === keyWord) {
    gameOver = true;
    return;
  }

  userWords.value.push(currentWord.value);
  currentTileIndex.value = 1;
  currentRowIndex.value += 1;
  currentWord.value = "";
}
function clearLetter() {
  currentWord.value = currentWord.value.slice(0, -1);

  currentTileIndex.value -= 1;
  tiles.value[currentRowIndex.value - 1][currentTileIndex.value - 1].letter =
    "";
}

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
      <div v-for="(row, i) in tiles" :key="i" class="board__row">
        <board-tile
          v-for="(tile, j) in row"
          :key="j"
          :letter="tile.letter"
          :color="tile.color"
        />
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
        <key-button char="enter" isWide @click="checkWord" />
        <key-button
          v-for="c in 'zxcvbnm'"
          :key="c"
          :char="c"
          @click="onKeyPressed(c)"
        />
        <key-button isWide isBackspace @click="clearLetter" />
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
