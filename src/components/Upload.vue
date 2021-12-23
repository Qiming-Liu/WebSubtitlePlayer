<script setup>
defineProps({
    msg: String
})
const selectedFile = document.getElementById('files').files[0];
</script>

<template>
    <input type="file" id="files">
</template>

<style scoped>
</style>
