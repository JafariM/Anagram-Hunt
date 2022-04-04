<template>
  <div class="container w-50">
    <div class="row">
      <h1 class="display1 text-center mb-5">Times Up!</h1>
    </div>
    <div class="row">
      <h3 class="display2 font-weight-bold text-center mb-5">You Got</h3>
    </div>
    <div class="row">
      <h3 class="display2 font-weight-bold text-center">{{ score }}</h3>
    </div>
    <div class="row">
      <h3 class="display3 font-weight-bold text-center mb-3 mt-4">Anagrams</h3>
    </div>
    <div class="row mb-3">
      <button class="btn btn-primary" @click="$emit('play-button-click')">
        Play Again
      </button>
    </div>
    <div class="row">
      <button class="btn btn-info" @click="$emit('start-again-click')">
        Back to Start Screen
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "GameResult",
  props: {
    score: Number,
  },
};
</script>