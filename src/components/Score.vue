<template>
  <div class="ml-4">
    <strong>Score:{{ value }}</strong>
  </div>
</template>
<script>
export default {
  name: "Score",
  props: {
    value: Number,
  },
};
</script>