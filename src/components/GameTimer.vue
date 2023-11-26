<template>
  <div class="stop-watch-container">
    <p class="stop-watch">
      <span>{{ hours ? `${zeroHor}${hours}` : `00` }}</span
      >:<span>{{ minutes ? `${zeroMin}${minutes}` : `00` }}</span
      >:<span>{{ secondes ? `${zero}${secondes}` : `00` }}</span>
    </p>
  </div>
</template>

<script setup>
import { onMounted, ref, watch } from 'vue'

// Props
const props = defineProps({
  stopTheTimer: {
    type: Boolean,
    required: true
  }
})

// Emitted events
const emits = defineEmits(['gameTime'])

// Data
const secondes = ref(0)
const minutes = ref(0)
const hours = ref(0)
const zero = ref('')
const zeroMin = ref('')
const zeroHor = ref('')

let timerId

// Functions
const updateTime = () => {
  secondes.value++
  if (secondes.value <= 9) {
    zero.value = '0'
  }
  if (secondes.value > 9) {
    zero.value = ''
  }
  if (secondes.value === 60) {
    minutes.value++
    secondes.value = 0
    zero.value = '0'
  }
  if (minutes.value <= 9) {
    zeroMin.value = '0'
  }
  if (minutes.value > 9) {
    zeroMin.value = ''
  }
  if (minutes.value === 60) {
    hours.value++
    minutes.value = 0
    zeroMin.value = '0'
  }
  if (hours.value <= 9) {
    zeroHor.value = '0'
  }
}

const stopTimer = () => {
  clearInterval(timerId)
}

// Watches
watch(
  () => props.stopTheTimer,
  (newValue) => {
    if (newValue) {
      stopTimer()
      emits('gameTime', hours.value, minutes.value, secondes.value)
    }
  }
)

onMounted(() => {
  timerId = setInterval(updateTime, 1000)
})
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Titan+One&display=swap');

.stop-watch-container {
  position: fixed;
  left: 0;
  top: 0;
  height: 33px;
  width: 100px;
  background-color: #212e3c;
  display: flex;
  align-items: center;
  justify-content: center;
  text-shadow: 1px 1px 1px #000;
}
.stop-watch {
  font-family: 'Titan One', cursive;
  color: #fff;
  font-weight: bold;
}
</style>
