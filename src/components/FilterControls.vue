<script setup lang="ts">
import { computed } from "vue";
import { useVModel } from "@vueuse/core";
import { Switch } from "@headlessui/vue";

const props = defineProps<{
  hideDoneTodos: boolean;
  sortByPriority: boolean;
}>();

const emit = defineEmits<{
  (e: "update:hideDoneTodos", value: boolean): void;
  (e: "update:sortByPriority", value: boolean): void;
}>();

/**
 * useVmodel is a shorthand for, documentation: https://vueuse.org/core/usevmodel/#usevmodel
 *
 * hideDoneTodos.value = true (in script tag) OR @click="hideDoneTodos = true" (in template tag) --> emit('update:hideDoneTodos', true)
 * and
 * hideDoneTodos.value = props.hideDoneTodos // wrapped in a ref, check the types
 *
 * We do this to update the hideDoneTodos ref that is handled in the parent component Todos.vue.
 */
const hideDoneTodos = useVModel(props, "hideDoneTodos", emit);
const sortByPriority = useVModel(props, "sortByPriority", emit);

const sortingBy = computed(() => {
  return `Sorting by ${props.sortByPriority ? "priority" : "status"}`;
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
