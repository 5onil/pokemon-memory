<template>
  <div
    @click="handleClick"
    class="pokemon-card"
    :class="[isFlipped || isAMatch ? 'flipped' : '', isAMatch ? 'match shake' : '']"
  >
    <div class="card-back">
      <img :src="backSideCard" alt="back side of card" />
    </div>
    <div class="card-front">
      <h2>{{ pokemon.name }}</h2>
      <img :src="pokemon.image" :alt="pokemon.name" />
    </div>
  </div>
</template>

<script setup>
import { ref, defineProps, watch, computed } from 'vue'
import backSideCard from '../assets/backsidecard.svg'

// props
const props = defineProps({
  pokemon: {
    type: Object,
    required: true
  },
  isMatched: {
    type: Array,
    required: true
  },
  resetCard: {
    type: Boolean,
    required: true
  }
})

// Data
const isFlipped = ref(false)

// Emitted events
const emits = defineEmits(['flip', 'sendBackReset'])

// Watcher
watch(
  () => props.resetCard,
  (newValue) => {
    if (newValue) {
      isFlipped.value = false
      emits('sendBackReset', false)
    }
  }
)

// Computed
const isAMatch = computed(() => {
  return props.isMatched.includes(props.pokemon.id)
})

// Handle click on card
const handleClick = () => {
  isFlipped.value = !isFlipped.value
  emits('flip', { isFlipped: isFlipped.value, pokemonId: props.pokemon.id })
}
</script>

<style scoped>
.flipped {
  pointer-events: none;
}
.match .card-front {
  box-shadow: 0px 0px 12px 3px rgb(225 172 31 / 87%);
}
.pokemon-card {
  width: 150px;
  height: 216px;
  flex: 1;
  flex-basis: auto;
  cursor: pointer;
  perspective: 1000px; /* This defines how far the 3D space is */
}

.card-front,
.card-back {
  position: absolute;
  width: 100%;
  height: 100%;
  border-radius: 6px;
  backface-visibility: hidden;
  transition: transform 0.6s ease-in-out; /* Smooth animation transition */
}
.card-back img {
  width: 100%;
  height: 100%;
  border-radius: 6px;
}
.match .card-front img {
  filter: drop-shadow(2px 3px 1px rgb(0 0 0 / 45%));
}
.card-front {
  width: 100%;
  height: 100%;
  border-radius: 6px;
  z-index: 2;
  display: flex;
  flex-direction: column;
  background-color: #ffffff;
}
.match .card-front {
  background-color: #eba64e;
}
.card-front h2 {
  border-radius: 6px 6px 0 0;
  text-align: center;
  background-color: burlywood;
}
.match .card-front h2 {
  border-radius: 6px 6px 0 0;
  background-color: #c48a40;
  text-shadow: 1px 1px 1px #000;
  color: #fff;
}

.card-front {
  transform: rotateY(180deg);
  z-index: 1;
  background-color: #f0f0f0; /* Change the background color for the back side */
}

.pokemon-card.flipped .card-back {
  transform: rotateY(180deg);
}

.pokemon-card.flipped .card-front {
  transform: rotateY(0deg);
}

.shake {
  animation: shakeAnimation 1s ease-in-out;
}

/* Animations */
@keyframes shakeAnimation {
  0% {
    transform: translateX(0);
  }
  10% {
    transform: translateY(-10px);
  }
  20% {
    transform: translateY(10px);
  }
  30% {
    transform: translateY(-10px);
  }
  40% {
    transform: translateY(10px);
  }
  50% {
    transform: translateY(-10px);
  }
  60% {
    transform: translateY(10px);
  }
  70% {
    transform: translateY(-10px);
  }
  80% {
    transform: translateY(10px);
  }
  90% {
    transform: translateY(-10px);
  }
  100% {
    transform: translateY(0);
  }
}
</style>
