<script setup lang="ts"></script>

<template>
  <main class="game__container">
    <div class="game__board">
      <div v-for="n in 6" :key="n" class="board__row">
        <div v-for="n in 'goody'" :key="n" class="board__tile">{{ n }}</div>
      </div>
    </div>
    <div class="keyboard__container">
      <div class="keyboard__row">
        <button type="button" class="key__button">q</button>
      </div>
    </div>
  </main>
</template>

<style scoped>
.game__container {
  margin-top: 65px;
  padding-top: 30px;
  box-sizing: border-box;
}
.game__board {
  width: 330px;
  height: 400px;
  display: grid;
  grid-template-rows: repeat(6, 1fr);
  gap: 5px;
  padding: 10px;
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
}
.keyboard__row {
}
.key__button {
}
</style>
