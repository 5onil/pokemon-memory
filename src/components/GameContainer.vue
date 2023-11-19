<template>
  <transition appear name="grow">
    <div class="pokemon-container" v-if="pokemons.length === 10">
      <BaseCard
        v-for="(pokemon, index) in pokemons"
        :key="index"
        :pokemon="pokemon"
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

const randomIds = () => {
  return Math.floor(Math.random() * 898) + 1
}

const fetchPokemon = async () => {
  // Create array with random ids
  const randomPokemonIds = Array.from({ length: 10 }, () => randomIds())

  for (let index = 0; index < randomPokemonIds.length; index++) {
    const id = randomPokemonIds[index]
    try {
      const response = await fetch(`https://pokeapi.co/api/v2/pokemon/${id}`)
      const pokemonData = await fetch(response.url)
      const pokemon = await pokemonData.json()

      if (pokemon) {
        pokemons.value.push({
          name: pokemon.name,
          image: pokemon.sprites.front_default
        })
      }
    } catch (error) {
      console.log(error)
    }
  }
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
