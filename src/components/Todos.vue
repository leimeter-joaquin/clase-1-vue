<script setup lang="ts">
import { v4 as uuidv4 } from "uuid";
import { reactive, ref, watch } from "vue";

interface Todos {
  id: string;
  description: string;
  priority: number;
  done: boolean;
}

const todos = ref<Todos[]>([
  {
    id: uuidv4(),
    description: "Go shoppping",
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

const newTodo = reactive({
  textInput: "",
  priotityInput: 0,
});

const add = (description: string, priority: number) => {
  if (newTodo.priotityInput < 0 || newTodo.priotityInput > 3) return;
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
    newTodo.textInput = "";
  }
};

const remove = (id: string) => {
  todos.value = todos.value.filter((v) => {
    return v.id !== id;
  });
};

const sortBy = ref<"priority" | "status">("status");
watch(sortBy, () => {
  if (sortBy.value === "priority") {
    todos.value.sort((a, b) => {
      if (a["priority"] < b["priority"]) {
        return 1;
      } else {
        return -1;
      }
    });
  } else if (sortBy.value === "status") {
    todos.value.sort((a) => {
      if (a.done) {
        return 1;
      } else {
        return -1;
      }
    });
  }
});
</script>

<template>
  <div class="w-[300px] m-auto mt-20">
    <h1 class="text-center text-3xl font-bold mb-1">Vue 3 To Do List</h1>
    <p class="text-center mb-5">Remaining ({{ todos.length }})</p>
    <ul class="space-y-2">
      <li
        v-for="item in todos"
        :key="item.id"
        class="flex justify-end items-center"
        :class="[{ 'line-through': item.done }]"
      >
        <p class="ml-2">{{ item.description }}</p>
        <button @click="remove(item.id)" class="rounded-full w-5 h-5 ml-5">
          x
        </button>
      </li>
    </ul>
    <div class="flex justify-around mt-5">
      <input v-model="newTodo.textInput" class="text-black outline-0 pl-2" />

      <label for="priority">Priority {{ newTodo.priotityInput }}</label>
      <input
        type="range"
        id="priority"
        name="Priority"
        min="1"
        max="4"
        v-model="newTodo.priotityInput"
      />

      <button
        @click="add(newTodo.textInput, newTodo.priotityInput)"
        class="rounded-lg px-2 py-1"
      >
        Add item
      </button>

      <label for="priority_sort_selector">Priority</label>
      <input
        id="priority_sort_selector"
        type="radio"
        v-model="sortBy"
        value="priority"
      />

      <label for="status_sort_selector">Status</label>
      <input
        id="status_sort_selector"
        type="radio"
        v-model="sortBy"
        value="status"
      />
    </div>
    <!-- <pre class="text-xs">{{ JSON.stringify(todos, null, 2) }}</pre> -->
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
