<template>
  <div class="win-bg" :class="{ 'win-bg-color': showWin }">
    <bg-confetti />
    <div v-if="showWin" class="win-container">
      <h1>Grattis du vann!</h1>
      <h2>Hurra!!</h2>
      <button @click="handleReload" class="reload-btn">Starta om</button>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue'
import BgConfetti from './BgConfetti.vue'

// Data
const showWin = ref(false)

// Functions
const handleReload = () => {
  window.location.reload()
}

const showDelay = () => {
  setTimeout(() => {
    showWin.value = true
  }, 3000)
}

onMounted(() => {
  showDelay()
})
</script>

<style scoped>
.win-bg {
  position: fixed;
  top: 0;
  bottom: 0;
  right: 0;
  left: 0;
}
.win-bg-color {
  background-color: rgba(0, 0, 0, 0.9);
  animation: bg-animation 1s ease-out;
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

/* Animation */
@keyframes bg-animation {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
</style>
