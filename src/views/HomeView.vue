<script setup lang="ts">
import { onMounted, onUnmounted, ref } from "vue";
import { Tile, Key } from "../types";
import wordsData from "../words.json";
import BoardTile from "../components/BoardTile.vue";
import KeyButton from "../components/KeyButton.vue";

const keyWord = "melon";
const todayDate = new Date().toDateString();

let isGameWin = false;

const createTile = (): Tile => ({
  letter: "",
  color: "transparent"
});
const createRow = (): Tile[] => Array.from({ length: 5 }, createTile);
const tiles = ref<Tile[][]>(Array.from({ length: 6 }, createRow));

const keyboard = ref<Key[]>(
  "qwertyuiopasdfghjklzxcvbnm".split("").map(char => ({
    char
  }))
);

const userWords = ref<string[]>([]);
const currentRowIndex = ref(0);
const currentTileIndex = ref(0);
const currentWord = ref("");

function onKeyPressed(c: string) {
  if (isGameWin) return;
  if (currentRowIndex.value >= 6) return;
  if (currentTileIndex.value >= 5) return;
  currentWord.value += c;
  tiles.value[currentRowIndex.value][currentTileIndex.value].letter = c;

  currentTileIndex.value += 1;
}

function checkWord(isLocalStorageCheck: boolean) {
  if (isGameWin) return;
  if (currentTileIndex.value !== 5) return;

  const isValidWord = wordsData.includes(currentWord.value);
  if (!isValidWord) {
    alert("Not in word list");
    return;
  }

  const keyWordDict = makeKeyWordDictionary();

  for (let i = 0; i < 5; i++) {
    const isExist = keyWordDict[currentWord.value[i]] > 0;
    const isMatch = currentWord.value[i] === keyWord[i];

    if (isExist) {
      keyWordDict[currentWord.value[i]] -= 1;
    }
    const color = isMatch ? "green" : isExist ? "yellow" : "gray";

    const tile = tiles.value[currentRowIndex.value][i];
    tile.color = color;

    const keyButton = keyboard.value.find(
      key => key.char === currentWord.value[i]
    )!;
    if (!keyButton.color || color === "green") {
      keyButton.color = color;
    }
  }
  if (!isLocalStorageCheck) {
    userWords.value.push(currentWord.value);
    localStorage.setItem(todayDate, JSON.stringify(userWords.value));
  }

  if (currentWord.value === keyWord) {
    isGameWin = true;
    return;
  }

  currentTileIndex.value = 0;
  currentRowIndex.value += 1;
  currentWord.value = "";
}

function makeKeyWordDictionary(): { [key: string]: number } {
  const keyWordDict: { [key: string]: number } = {};
  for (let char of keyWord) {
    keyWordDict[char] = (keyWordDict[char] || 0) + 1;
  }
  return keyWordDict;
}

function clearLetter() {
  if (!currentWord.value || isGameWin) return;
  currentWord.value = currentWord.value.slice(0, -1);

  currentTileIndex.value -= 1;
  tiles.value[currentRowIndex.value][currentTileIndex.value].letter = "";
}

const keyboardListener = (e: KeyboardEvent) => {
  if (e.code.includes("Key")) {
    onKeyPressed(e.key);
  } else if (e.code === "Backspace") {
    clearLetter();
  } else if (e.code === "Enter") {
    checkWord(false);
  }
};

function checkTodayAttempts() {
  const stringifiedAttempts = localStorage.getItem(todayDate);
  if (stringifiedAttempts) {
    const attempts = JSON.parse(stringifiedAttempts);
    userWords.value = attempts;
    console.log(attempts);

    for (const word of attempts) {
      currentWord.value = word;
      for (let i = 0; i < 5; i++) {
        tiles.value[currentRowIndex.value][i].letter = word[i];
      }
      currentTileIndex.value = 5;
      checkWord(true);
    }
  } else {
    localStorage.clear();
  }
}

onMounted(() => {
  window.addEventListener("keydown", keyboardListener);
  checkTodayAttempts();
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
          :delay="j * 100"
          :playWinAnimation="isGameWin && currentRowIndex === i"
        />
      </div>
    </div>
    <div class="keyboard__container">
      <div class="keyboard__row">
        <key-button
          v-for="key in keyboard.slice(0, 10)"
          :key="key.char"
          :char="key.char"
          :color="key.color"
          @click="onKeyPressed(key.char)"
        />
      </div>
      <div class="keyboard__row">
        <key-button
          v-for="key in keyboard.slice(10, 19)"
          :key="key.char"
          :char="key.char"
          :color="key.color"
          @click="onKeyPressed(key.char)"
        />
      </div>
      <div class="keyboard__row">
        <key-button char="enter" isWide @click="checkWord(false)" />
        <key-button
          v-for="key in keyboard.slice(19, 26)"
          :key="key.char"
          :char="key.char"
          :color="key.color"
          @click="onKeyPressed(key.char)"
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
