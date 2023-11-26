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
      <div class="sign">
        <span class="fast-flicker">D</span>in<span class="flicker"> tid:</span
        ><span class="fast-flicker">{{ gameTime[0] != 0 ? ` ${gameTime[0]} timme ` : '' }}</span
        ><span class="flicker">{{ gameTime[1] != 0 ? ` ${gameTime[1]} minuter ` : '' }}</span
        >{{ gameTime[2] }} sekunder
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue'
import BgConfetti from './BgConfetti.vue'
import ButtonWon from './ButtonWon.vue'
import wonSoundFile from './audio/won-sound.mp3'

// Data
const showWin = ref(false)
const wonSound = new Audio(wonSoundFile)

// Props
const props = defineProps({
  gameTime: {
    type: Array,
    required: true
  }
})

// Functions
const handleReload = () => {
  window.location.reload()
}

const showDelay = () => {
  setTimeout(() => {
    wonSound.loop = true
    wonSound.play()
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

/* Din tid */
.sign {
  margin-top: 30px;
  background-image: radial-gradient(ellipse 50% 35% at 50% 50%, #6b1839, transparent);
  font-family: 'Titan One', cursive;
  text-transform: uppercase;
  font-size: 2em;
  color: #ffe6ff;
  text-shadow: 0 0 0.6rem #ffe6ff, 0 0 1.5rem #ff65bd, -0.2rem 0.1rem 1rem #ff65bd,
    0.2rem 0.1rem 1rem #ff65bd, 0 -0.5rem 2rem #ff2483, 0 0.5rem 3rem #ff2483;
  animation: shine 2s forwards, flicker 3s infinite;
}

@keyframes blink {
  0%,
  22%,
  36%,
  75% {
    color: #ffe6ff;
    text-shadow: 0 0 0.6rem #ffe6ff, 0 0 1.5rem #ff65bd, -0.2rem 0.1rem 1rem #ff65bd,
      0.2rem 0.1rem 1rem #ff65bd, 0 -0.5rem 2rem #ff2483, 0 0.5rem 3rem #ff2483;
  }
  28%,
  33% {
    color: #ff65bd;
    text-shadow: none;
  }
  82%,
  97% {
    color: #ff2483;
    text-shadow: none;
  }
}

.flicker {
  animation: shine 2s forwards, blink 3s 2s infinite;
}

.fast-flicker {
  animation: shine 2s forwards, blink 10s 1s infinite;
}

@keyframes shine {
  0% {
    color: #6b1839;
    text-shadow: none;
  }
  100% {
    color: #ffe6ff;
    text-shadow: 0 0 0.6rem #ffe6ff, 0 0 1.5rem #ff65bd, -0.2rem 0.1rem 1rem #ff65bd,
      0.2rem 0.1rem 1rem #ff65bd, 0 -0.5rem 2rem #ff2483, 0 0.5rem 3rem #ff2483;
  }
}

@keyframes flicker {
  from {
    opacity: 1;
  }

  4% {
    opacity: 0.9;
  }

  6% {
    opacity: 0.85;
  }

  8% {
    opacity: 0.95;
  }

  10% {
    opacity: 0.9;
  }

  11% {
    opacity: 0.922;
  }

  12% {
    opacity: 0.9;
  }

  14% {
    opacity: 0.95;
  }

  16% {
    opacity: 0.98;
  }

  17% {
    opacity: 0.9;
  }

  19% {
    opacity: 0.93;
  }

  20% {
    opacity: 0.99;
  }

  24% {
    opacity: 1;
  }

  26% {
    opacity: 0.94;
  }

  28% {
    opacity: 0.98;
  }

  37% {
    opacity: 0.93;
  }

  38% {
    opacity: 0.5;
  }

  39% {
    opacity: 0.96;
  }

  42% {
    opacity: 1;
  }

  44% {
    opacity: 0.97;
  }

  46% {
    opacity: 0.94;
  }

  56% {
    opacity: 0.9;
  }

  58% {
    opacity: 0.9;
  }

  60% {
    opacity: 0.99;
  }

  68% {
    opacity: 1;
  }

  70% {
    opacity: 0.9;
  }

  72% {
    opacity: 0.95;
  }

  93% {
    opacity: 0.93;
  }

  95% {
    opacity: 0.95;
  }

  97% {
    opacity: 0.93;
  }

  to {
    opacity: 1;
  }
}
</style>
