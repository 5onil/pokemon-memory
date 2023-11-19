<template>
  <div @click="handleClick" class="pokemon-card" :class="isFlipped || isAMatch ? 'flipped' : ''">
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

const { pokemon } = props

// Data
const isFlipped = ref(false)

// Emitted events
const emits = defineEmits(['flip', 'sendBackReset'])

// Watcher
watch(
  () => props.resetCard,
  (newValue, oldValue) => {
    if (newValue) {
      isFlipped.value = false
      emits('sendBackReset', false)
    }
  }
)

const isAMatch = computed(() => {
  return isFlipped && props.isMatched.includes(pokemon.id)
})

// Handle click on card
const handleClick = () => {
  isFlipped.value = !isFlipped.value
  emits('flip', { isFlipped: isFlipped.value, pokemonId: pokemon.id })
}
</script>

<style scoped>
.match .card-front {
  border: 4px solid red;
}
.pokemon-card {
  width: 150px;
  height: 216px;
  flex: 1;
  flex-basis: 15%;
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
.card-front {
  width: 100%;
  height: 100%;
  border-radius: 6px;
  z-index: 2;
  display: flex;
  flex-direction: column;
  background-color: #ffffff;
}
.card-front h2 {
  border-radius: 6px 6px 0 0;
  text-align: center;
  background-color: burlywood;
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
</style>
