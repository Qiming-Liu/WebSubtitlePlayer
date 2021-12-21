<script setup>
defineProps({
  msg: String
})
</script>

<template>
  <div>
    <slot />
  </div>
</template>

<style scoped>
div {
  background-color: #000000;
  height: 96vh;
  width: 100vw;
  display : flex;
  align-items : center;
}
</style>
