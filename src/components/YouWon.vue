<template>
  <div class="win-bg">
    <bg-confetti />
    <div class="win-container">
      <h1>Grattis du vann!</h1>
      <h2>Hurra!!</h2>
      <button @click="handleReload" class="reload-btn">Starta om</button>
    </div>
  </div>
</template>

<script setup>
import BgConfetti from './BgConfetti.vue'

// Functions
const handleReload = () => {
  window.location.reload()
}
</script>

<style scoped>
.win-bg {
  background-color: rgba(0, 0, 0, 0.9);
  position: fixed;
  top: 0;
  bottom: 0;
  right: 0;
  left: 0;
}
.win-container {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  color: #ffffff;
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
}

.reload-btn {
  margin-top: 20px;
  padding: 20px 40px;
  background-color: teal;
  color: #ffffff;
  border: none;
  border-radius: 40px;
  cursor: pointer;
}
</style>
