<script setup lang="ts">
defineProps<{
  remainingTodos?: number;
}>();
</script>

<template>
  <div class="text-center flex flex-col mb-5 gap-1">
    <h1 class="text-3xl font-bold">Vue 3 To Do List</h1>
    <p class="">Remaining ({{ remainingTodos }})</p>
  </div>
</template>

<style scoped></style>
