<script setup lang="ts">
import { XMarkIcon } from "@heroicons/vue/24/solid";
import { Todo } from "../types";

const props = defineProps<{
  todoItem: Todo;
}>();

const emits = defineEmits<{
  (e: "removeItem", id: string): void;
}>();
</script>

<template>
  <span
    class="overflow-ellipsis"
    :class="[{ 'line-through': todoItem.done }]"
    @click="todoItem.done = !todoItem.done"
    >{{ todoItem.description }}</span
  >
  <span>{{ todoItem.priority }}</span>
  <button
    @click="emits('removeItem', todoItem.id)"
    class="rounded-full w-5 h-5 flex items-center justify-center group"
  >
    <XMarkIcon class="h-0 group-hover:h-4" />
  </button>
</template>

<style scoped>
</style>