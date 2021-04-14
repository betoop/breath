<template>
  <div class="">

  </div>
  <button class="border border-black rounded px-2" @click="start" >Start</button>

  {{ counter }}
</template>



<script setup>
  import { ref } from 'vue'

  
  const started = ref(false)
  let interval

  const matrix = [
    [5, 11, 16],
    // [21, 27, 32],
    // [37, 44, 49],
    // [54, 1, 6],
    // [11, 19, 24],
    // [29, 37, 42],
    // [47, 56, 1],
    [6, 16, 21],
    // [26, 37, 42],
    // [47, 59, 4]
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
      return
    }

    interval = setInterval(() => {
      started.value = true
      counter.value--

      if (counter.value < 0) {
        
        if (action.value < EXHALE+1) {
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
    }, 100);
  }
</script>