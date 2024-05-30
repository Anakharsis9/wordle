<script setup lang="ts"></script>

<template>
  <main class="game__container">
    <div class="game__board">
      <div v-for="n in 6" :key="n" class="board__row">
        <div v-for="n in 'goody'" :key="n" class="board__tile"></div>
      </div>
    </div>
    <div class="keyboard__container">
      <div class="keyboard__row">
        <button
          v-for="n in 'qwertyuiop'"
          :key="n"
          type="button"
          class="key__button"
        >
          {{ n }}
        </button>
      </div>
      <div class="keyboard__row">
        <button
          v-for="n in 'asdfghjkl'"
          :key="n"
          type="button"
          class="key__button"
        >
          {{ n }}
        </button>
      </div>
      <div class="keyboard__row">
        <button type="button" class="key__button key__button-wide">
          enter
        </button>
        <button
          v-for="n in 'zxcvbnm'"
          :key="n"
          type="button"
          class="key__button"
        >
          {{ n }}
        </button>
        <button type="button" class="key__button key__button-wide">
          <img src="/src/assets/icons/backspace-icon.svg" alt="backspace" />
        </button>
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
}
.key__button-wide {
  flex: 0.9;
  font-size: 12px;
}
</style>
