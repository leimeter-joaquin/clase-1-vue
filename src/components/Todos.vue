<script setup lang="ts">
import { v4 as uuidv4 } from "uuid";
import { Switch } from "@headlessui/vue";
import { computed, ref, watchEffect } from "vue";

interface Todos {
  id: string;
  description: string;
  priority: number;
  done: boolean;
}

const todos = ref<Todos[]>([
  {
    id: uuidv4(),
    description: "Go shopping",
    priority: 1,
    done: true,
  },
  {
    id: uuidv4(),
    description: "Call mom",
    priority: 3,
    done: false,
  },
]);

// const newTodo = reactive({
//   textInput: "",
//   priotityInput: 1,
// });

const textInput = ref("");
const priorityInput = ref(1);

const add = (description: string, priority: number) => {
  if (priorityInput.value < 1 || priorityInput.value > 3) return;
  if (description) {
    todos.value = [
      ...todos.value,
      {
        id: uuidv4(),
        description,
        priority,
        done: false,
      },
    ];
    textInput.value = "";
  }
};

const remove = (id: string) => {
  todos.value = todos.value.filter((v) => {
    return v.id !== id;
  });
};

const sortByPiority = ref(false);
watchEffect(() => {
  if (sortByPiority.value) {
    todos.value.sort((a, b) => b.priority - a.priority);
  } else {
    todos.value.sort((a) => (a.done ? 1 : -1));
  }
});

const remainingTodos = computed(() => {
  return todos.value.filter((t) => {
    return !t.done;
  }).length;
});
const sortBy = computed(() => {
  return `Sort by ${sortByPiority.value ? "priority" : "status"}`;
});
</script>
<template>
  <div class="flex mt-20 items-start justify-center gap-4">
    <div class="w-[300px] bg-[#333] p-2 rounded">
      <div class="text-center flex flex-col mb-5 gap-1">
        <h1 class="text-3xl font-bold">Vue 3 To Do List</h1>
        <p class="">Remaining ({{ remainingTodos }})</p>
      </div>

      <div class="flex justify-end p-2 mb-3 rounded bg-[#35485e] gap-8">
        <p class="text-right">Sort By</p>
        <div class="flex gap-4">
          <span>status</span>
          <Switch
            v-model="sortByPiority"
            class="relative inline-flex h-6 w-11 items-center rounded-full bg-white"
          >
            <span class="sr-only">{{ sortBy }}</span>
            <span
              :class="sortByPiority ? 'translate-x-6' : 'translate-x-1'"
              class="inline-block h-4 w-4 transform rounded-full bg-[#242424] transition"
            />
          </Switch>
          <span>priority</span>
        </div>
      </div>
      <ul v-auto-animate class="space-y-2 flex flex-col items-end">
        <li
          v-for="item in todos"
          :key="item.id"
          class="flex justify-end items-center gap-2 cursor-pointer"
          @click="item.done = !item.done"
        >
          <span class="" :class="[{ 'line-through': item.done }]">{{
            item.description
          }}</span>
          <span class="">{{ item.priority }}</span>
          <button @click="remove(item.id)" class="rounded-full w-5 h-5">
            x
          </button>
        </li>
      </ul>
      <div
        class="flex flex-col items-center gap-2 justify-around mt-5 bg-[#35485e] p-2 pt-4 rounded"
      >
        <input v-model="textInput" class="text-black outline-0 pl-2" />
        <div class="flex gap-2">
          <label for="priority">Priority {{ priorityInput }}</label>
          <input
            type="range"
            id="priority"
            name="Priority"
            min="1"
            max="3"
            v-model="priorityInput"
            class="text-green-300 bg-green-500"
          />
        </div>
        <button
          @click="add(textInput, priorityInput)"
          class="rounded-lg px-2 py-1"
        >
          Add item
        </button>
      </div>
      <pre class="text-xs mt-4">{{
        JSON.stringify({ textInput, priorityInput }, null, 2)
      }}</pre>
    </div>
    <pre class="text-xs">{{ JSON.stringify(todos, null, 2) }}</pre>
  </div>
</template>

<style scoped>
button {
  background-color: rgba(209, 213, 219);
  color: black;
}

button:hover {
  background-color: rgba(156, 163, 175);
}
</style>
