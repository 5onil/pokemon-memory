<template>
  <div class="win-bg" :class="{ 'win-bg-color': showWin }">
    <bg-confetti />
    <div v-if="showWin" class="win-container">
      <div class="wrapper">
        <span>G</span>
        <span>R</span>
        <span>A</span>
        <span>T</span>
        <span>T</span>
        <span>I</span>
        <span>S</span>
      </div>
      <button-won @click.prevent="handleReload" />
      <h2>Hurra!!</h2>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue'
import BgConfetti from './BgConfetti.vue'
import ButtonWon from './ButtonWon.vue'

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
@import url('https://fonts.googleapis.com/css2?family=Titan+One&display=swap');
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

.win-container h2 {
  margin-top: 30px;
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

.wrapper {
  position: relative;
  top: -140px;
  width: 100vw;
  text-align: center;
}
.wrapper span {
  -webkit-text-stroke-width: 1.25px;
  -webkit-text-stroke-color: #000;
  font-size: 100px;
  text-shadow: 0 0px #f3c623, 0 0px #f2aaaa;
  transform: translate(0, 100%) rotate(4deg);
  animation: jump 2s ease-in-out infinite;
  display: inline-block;
  font-family: 'Titan One', cursive;
  color: #fff;
}
.wrapper span:nth-child(1) {
  animation-delay: 120ms;
}
.wrapper span:nth-child(2) {
  animation-delay: 240ms;
}
.wrapper span:nth-child(3) {
  animation-delay: 360ms;
}
.wrapper span:nth-child(4) {
  animation-delay: 480ms;
}
.wrapper span:nth-child(5) {
  animation-delay: 600ms;
}
.wrapper span:nth-child(6) {
  animation-delay: 720ms;
}
.wrapper span:nth-child(7) {
  animation-delay: 840ms;
}

@keyframes jump {
  33% {
    text-shadow: 0 60px #f37121, 0 150px #f2aaaa;
  }
  50% {
    transform: translate(0, 0) rotate(-4deg);
    text-shadow: 0 0px #8fc0a9, 0 0px #84a9ac;
  }
  66.67% {
    text-shadow: 0 -60px #d54062, 0 -150px #8fc0a9;
  }
}

@media screen and (max-height: 200px) {
  .wrapper {
    transform: scale(0.3, 0.3);
  }
}
@media screen and (max-width: 576px) {
  .wrapper span {
    font-size: 50px;
  }
}
</style>
