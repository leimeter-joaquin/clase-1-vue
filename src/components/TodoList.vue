<script setup lang="ts">
import { Todo } from "../types";
import { XMarkIcon } from "@heroicons/vue/24/solid";
import TodoItem from "./TodoItem.vue"

const props = defineProps<{
  todos?: Todo[] | null;
  shownTodos?: Todo[] | null;
}>();

const emits = defineEmits<{
  (e: "remove", id: string): void;
}>();

const removeItem = (id:string) => {
  emits('removeItem', id)
}
</script>
<template>
  <ul
    v-auto-animate
    class="space-y-2 flex flex-col items-end py-4"
    v-if="todos"
  >
    <li
      v-for="item in shownTodos"
      :key="item.id"
      class="flex justify-end items-center gap-2 cursor-pointer truncate"
    >
      <TodoItem :todoItem="item" @removeItem="removeItem($event)" />
    </li>
  </ul>
  <span v-else>...Loading</span>
</template>
<style scoped></style>
