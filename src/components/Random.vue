<script setup lang="ts">
import { ref } from 'vue'

// reactive state
const count = ref(0)
const currentNumber = ref(0)
const numbers = ref([1])
const previousNumbers = ref<number[]>([])
function getRandomInt(max: number) {
  return Math.floor(Math.random() * max)
}

function onChange(event: Event) {
  numbers.value = Array.from(
    { length: parseInt((event.target as HTMLInputElement).value) },
    (_, i) => i + 1
  )
  currentNumber.value = 0
  console.log(numbers.value.length)
}
function getNewNumber() {
  if (numbers.value.length > 0) {
    const index = getRandomInt(numbers.value.length)
    currentNumber.value = numbers.value[index]
    numbers.value.splice(index, 1)
    console.log(numbers.value.length)
    previousNumbers.value.push(currentNumber.value)
    console.log(currentNumber.value)
    if (previousNumbers.value.length > 5) {
      previousNumbers.value.splice(0, 1)
    }
  }
}
</script>

<template>
  <div class="greetings">
    <label for="count">Enter count: </label>
    <input name="count" v-model="count" placeholder="100" type="number" v-on:change="onChange" />
    <br />
    <br />
    <button v-on:click="getNewNumber">New number</button>
    <p>Current: {{ currentNumber }}</p>
    <li v-for="number in previousNumbers.slice().reverse()" v-bind:key="number">
      {{ number }}
    </li>
  </div>
</template>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  position: relative;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>
