<template>
  <div>
    <h4>{{ word }}({{ remain - 1 }} left)</h4>
  </div>
</template>
<script>
export default {
  name: "Anagram",
  props: {
    word: String,
    remain: Number,
  },
};
</script>