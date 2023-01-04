<script setup lang="ts">
import { computed, ref, watch, watchEffect } from "vue";
import { useVModel } from "@vueuse/core";
import { Switch } from "@headlessui/vue";
import { Todos } from "../types";

const props = defineProps<{
  todos: Todos[] | null;
  shownTodos: Todos[] | null;
}>();

const emits = defineEmits<{
  (e: "update:shownTodos"): void;
}>();

const shownTodos = useVModel(props, "shownTodos", emits);

const sortByPriority = ref(false);
const hideDoneTodos = ref(false);

watch(sortByPriority, () => {
  shownTodos.value = sortByPriority.value
    ? shownTodos.value?.sort((a, b) => b.priority - a.priority)
    : shownTodos.value?.sort((a) => (a.done ? 1 : -1))
});

watchEffect(() => {
  shownTodos.value = hideDoneTodos.value
    ? props.todos?.filter((t) => !t.done) ?? null
    : props.todos
});

const sortingBy = computed(() => {
  return `Sorting by ${sortByPriority.value ? "priority" : "status"}`;
});
</script>

<template>
  <div
    class="flex justify-end items-center px-2 py-4 rounded bg-[#303030] rounded-lg gap-4"
  >
    <button
      class="py-1 px-3 rounded-xl"
      @click="hideDoneTodos = !hideDoneTodos"
    >
      {{ !hideDoneTodos ? "Hide" : "Show" }} done todos
    </button>
    <p class="text-right">Sort By:</p>
    <div class="flex items-center gap-2">
      <span class="font-bold">status</span>
      <Switch
        v-model="sortByPriority"
        class="relative inline-flex h-6 w-11 items-center rounded-full bg-white"
      >
        <span class="sr-only">{{ sortingBy }}</span>
        <span
          :class="sortByPriority ? 'translate-x-6' : 'translate-x-1'"
          class="inline-block h-4 w-4 transform rounded-full bg-[#242424] transition"
        />
      </Switch>
      <span class="font-bold">priority</span>
    </div>
  </div>
</template>
<style scoped></style>
