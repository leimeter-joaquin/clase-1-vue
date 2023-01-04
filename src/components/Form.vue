<script setup lang="ts">
import { ref } from "vue";

const textInput = ref("");
const priorityInput = ref(1);

const emits = defineEmits<{
  (
    e: "add",
    { description, priority }: { description: string; priority: number }
  ): void;
}>();

const handleAdd = (description: string, priority: number) => {
  emits("add", { description, priority });
  textInput.value = "";
  priorityInput.value = 1;
};
</script>
<template>
  <div
    class="flex flex-col items-center gap-2 justify-around bg-[#303030] rounded-lg px-2 py-4 rounded"
  >
    <input
      v-model.trim="textInput"
      class="text-black outline-0 rounded-md pl-2"
    />
    <div class="flex gap-2">
      <label for="priority">Priority {{ priorityInput }}</label>
      <input
        type="range"
        id="priority"
        name="Priority"
        min="1"
        max="3"
        v-model.number="priorityInput"
      />
    </div>
    <button
      @click="handleAdd(textInput, priorityInput)"
      class="px-2 py-1 rounded-xl"
    >
      Add item
    </button>

    <pre class="text-xs mt-4">{{
      JSON.stringify({ textInput, priorityInput }, null, 2)
    }}</pre>
  </div>
</template>
<style scoped></style>
