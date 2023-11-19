<script setup>
import HelloWorld from './components/HelloWorld.vue'
import TheWelcome from './components/TheWelcome.vue'
import GameContainer from './components/GameContainer.vue'
</script>

<template>
  <header>
    <h1>Pokemons Memory</h1>
  </header>

  <main>
    <GameContainer />
  </main>
</template>

<style scoped>
h1 {
  color: white;
}
</style>
