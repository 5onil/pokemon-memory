<template>
  <transition appear name="grow">
    <div class="pokemon-container" v-if="pokemons.length === 10">
      <BaseCard
        v-for="(pokemon, index) in pokemons"
        :key="index"
        :pokemon="pokemon"
        @flip="handleIsFlipped"
        class="base-card"
      />
    </div>
  </transition>
</template>

<script setup>
import BaseCard from './BaseCard.vue'
import { ref, onMounted } from 'vue'

// Data
const pokemons = ref([])
const flippedCard = ref([])

// Create random ids
const randomIds = () => {
  return Math.floor(Math.random() * 898) + 1
}

// Fetch pokemon data from api
const fetchPokemon = async () => {
  let tempPokemons = []
  // Create array with random ids
  const randomPokemonIds = Array.from({ length: 5 }, () => randomIds())

  for (let index = 0; index < randomPokemonIds.length; index++) {
    const id = randomPokemonIds[index]
    try {
      const response = await fetch(`https://pokeapi.co/api/v2/pokemon/${id}`)
      const pokemonData = await fetch(response.url)
      const pokemon = await pokemonData.json()

      if (pokemon) {
        tempPokemons.push({
          id: pokemon.id,
          name: pokemon.name,
          image: pokemon.sprites.front_default
        })
      }
    } catch (error) {
      console.log(error)
    }
  }
  pokemons.value = duplicateAndMergePokemons(tempPokemons)
}

// Duplicate and randomize order of pokemons
const duplicateAndMergePokemons = (arr) => {
  const duplicatedArr = [...arr] // Duplicate the original array
  const mergedArr = [...arr, ...duplicatedArr] // Merge both arrays

  // Shuffle the merged array in a different random order
  for (let i = mergedArr.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1))
    ;[mergedArr[i], mergedArr[j]] = [mergedArr[j], mergedArr[i]]
  }

  return mergedArr
}

// Handle flipped cards
const handleIsFlipped = (value) => {
  if (flippedCard.value.length <= 1 && value.isFlipped) {
    flippedCard.value.push(value.pokemonId)
  }
  console.log(flippedCard.value)
  if (flippedCard.value.length === 2) {
    console.log('cards match?', cardsMatch())
    flippedCard.value = []
  }
}

// Checks if the cards are a match
const cardsMatch = () => {
  return flippedCard.value[0] === flippedCard.value[1] ? true : false
}

onMounted(() => {
  fetchPokemon()
})
</script>

<style scoped>
.pokemon-container {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}

.grow-enter-active {
  animation: grow 2s;
}

@keyframes grow {
  from {
    transform: scale(0);
  }
  to {
    transform: scale(1);
  }
}
</style>
