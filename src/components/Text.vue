<script setup>
defineProps({
  size: String,
  msg: String
})
</script>
<template>
  <p :style="'font-size:' + size + 'rem;'">{{ msg }}</p>
</template>

<style scoped>
p {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  color: #ffffff;
  font-weight: bold;
  margin: auto;
}
</style>
