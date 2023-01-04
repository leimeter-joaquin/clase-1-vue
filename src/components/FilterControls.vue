<script setup lang="ts">
import { computed, ref, watch, watchEffect } from "vue";
import { Switch } from "@headlessui/vue";

const props = defineProps<{
  hideDoneTodos: boolean;
  sortByPriority: boolean;
}>();

const emit = defineEmits<{
  (e: "toggleDoneFilter"): void;
  (e: "togglePriorityFilter"): void;
}>();

const sortingBy = computed(() => {
  return `Sorting by ${props.sortByPriority ? "priority" : "status"}`;
});
</script>

<template>
  <div
    class="flex justify-end items-center px-2 py-4 rounded bg-[#303030] rounded-lg gap-4"
  >
    <button class="py-1 px-3 rounded-xl" @click="emit('toggleDoneFilter')">
      {{ !hideDoneTodos ? "Hide" : "Show" }} done todos
    </button>
    <p class="text-right">Sort By:</p>
    <div class="flex items-center gap-2">
      <span class="font-bold">status</span>
      <Switch
        @click="emit('togglePriorityFilter')"
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
