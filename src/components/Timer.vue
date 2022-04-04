<template>
  <div class="mr-4">
    <Strong>Timer: {{ timeLeft }}</Strong>
  </div>
</template>
<script>
export default {
  name: "Timer",
  props: {
    timeLeft: Number,
  },
};
</script>