<script setup>
import { ref } from 'vue'

const curr = ref(0)
const best = ref(0)
</script>

<template>
  <div>
    <div>
      <h2>Current Score : {{ curr }}</h2>
      <h2>Best Score : {{ best }}</h2>
    </div>
    <div></div>
    <div>
      <button>Reset</button>
    </div>
  </div>
</template>
