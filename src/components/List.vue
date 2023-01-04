<script setup lang="ts">
import { Todos } from "../types";
import { XMarkIcon } from "@heroicons/vue/24/solid";

const props = defineProps<{
  todos?: Todos[] | null;
  shownTodos?: Todos[] | null;
}>();

const emits = defineEmits<{
  (e: "remove", id: string): void;
}>();
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
      @click="item.done = !item.done"
    >
      <span
        class="overflow-ellipsis"
        :class="[{ 'line-through': item.done }]"
        >{{ item.description }}</span
      >
      <span class="">{{ item.priority }}</span>
      <button
        @click="emits('remove', item.id)"
        class="rounded-full w-5 h-5 flex items-center justify-center"
      >
        <XMarkIcon class="h-4" />
      </button>
    </li>
  </ul>
  <span v-else>...Loading</span>
</template>
<style scoped></style>
