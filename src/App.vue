<script setup lang="ts">
import PoetryQuiz from './components/PoetryQuiz.vue'
</script>

<template>
  <header>
      <h1>Poem or Nah</h1>
      <h2>Two of these are real poems. One was written by AI. Can you guess which one is the fake?</h2>
  </header>
  <PoetryQuiz />
</template>

<style scoped>
header {
  background-color: lightsteelblue;
  color: black;
  padding: 25px;
  text-align: center;
  margin: 2.5% auto;
  position: relative;
  border:5px solid lightslategray;
  border-radius: 25px;
}
</style>
