<template>
  <div class="flex h-screen justify-center items-center mt-6 flex-col">
    <div class="flex">
      <template v-for="(_m, r) in matrix">
        <div v-for="(_a, a) in _m"
             :class="{
          'bg-indigo-500': a === BREATH,
          'bg-purple-400': a === HOLD,
          'bg-yellow-400': a === EXHALE,
          'border-4 border-gray-200': row === r && a === action
        }"
             class="w-4 h-6 block mx-0.5 rounded">
        </div>
      </template>
    </div>

    <div :class="{'opacity-30': !started}" class="text-4xl my-8 text-gray-600">
      <template v-if="action === BREATH">
        Inhalar 😤
      </template>
      <template v-if="action === HOLD">
        Mantener 🤭
      </template>
      <template v-if="action === EXHALE">
        Exhalar 😮‍💨
      </template>
    </div>

    <div
        :class="{
          'opacity-30': !started,
          'text-indigo-600': action === BREATH,
          'text-purple-700': action === HOLD,
          'text-yellow-600': action === EXHALE,
        }"
        class="text-9xl">{{ counter }}
    </div>

    <div class="mt-8">
      <button class="border border-gray-300 rounded text-2xl text-gray-500 py-2 px-6 hover:bg-gray-300" @click="start">
        {{ started ? 'Stop' : 'Start' }}
      </button>
    </div>

  </div>
</template>


<script setup>
import { ref } from 'vue'


const started = ref(false)
let interval

const matrix = [
  [5, 11, 16],
  [21, 27, 32],
  [37, 44, 49],
  [54, 1, 6],
  [11, 19, 24],
  [29, 37, 42],
  [47, 56, 1],
  [6, 16, 21],
  [26, 37, 42],
  [47, 59, 4]
]

const BREATH = 0
const HOLD = 1
const EXHALE = 2

let row = ref(0)
let action = ref(BREATH)

const counter = ref(matrix[0][BREATH])

function start() {
  if (started.value) {
    clearInterval(interval)
    counter.value = matrix[0][BREATH]
    started.value = false
    row.value = 0
    action.value = BREATH
    return
  }
  started.value = true

  interval = setInterval(() => {
    counter.value--

    if (counter.value < 0) {

      if (action.value < EXHALE + 1) {
        action.value++
      }

      if (action.value > EXHALE) {
        row.value++
        action.value = BREATH
      }

      if (row.value > matrix.length - 1) {
        row.value = 0
        action.value = BREATH
      }

      counter.value = matrix[row.value][action.value]
    }
  }, 1000);
}
</script>
