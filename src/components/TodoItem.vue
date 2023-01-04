<script setup lang="ts">
import { XMarkIcon } from "@heroicons/vue/24/solid";
import { Todo } from "../types";

const props = defineProps<{
  todoItem: Todo;
}>();

const emits = defineEmits<{
  (e: "removeItem", id: Todo): void;
}>();
</script>

<template>
  <li class="flex justify-end items-center gap-2 cursor-pointer truncate">
    <span
      class="overflow-ellipsis"
      :class="[{ 'line-through': todoItem.done }]"
      @click="todoItem.done = !todoItem.done"
      >{{ todoItem.description }}</span
    >
    <span>{{ todoItem.priority }}</span>
    <button
      @click="emits('removeItem', todoItem)"
      class="rounded-full w-5 h-5 flex items-center justify-center group"
    >
      <XMarkIcon class="h-0 group-hover:h-4" />
    </button>
  </li>
</template>

<style scoped></style>
