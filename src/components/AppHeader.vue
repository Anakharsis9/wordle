<script setup lang="ts"></script>

<template>
  <header class="header">
    <div class="header__container">
      <a class="icon__link" href="/">
        <img src="/wordle-seeklogo.svg" alt="logo" class="logo" />
      </a>
      <a class="icon__link help__icon">
        <img src="/src/assets/icons/help-icon.svg" alt="help-icon" />
      </a>
    </div>
  </header>
</template>

<style scoped>
.header {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 65px;
  border-bottom: 1px solid rgba(58, 58, 60, 0.8);
}

.header__container {
  display: flex;
  align-items: center;
  justify-content: center;
}

.icon__link {
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
}

.logo {
  display: block;
  width: 105px;
  height: 60px;
  object-fit: contain;
}

.help__icon {
  position: absolute;
  right: 50px;
}
</style>
