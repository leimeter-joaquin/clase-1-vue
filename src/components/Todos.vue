<script setup lang="ts">
import { v4 as uuidv4 } from "uuid";
import { computed, onMounted, ref, watch, watchEffect } from "vue";
import axios from "axios";
import { Todo } from "../types";
import Header from "./Header.vue";
import FilterControls from "./FilterControls.vue";
import TodoList from "./TodoList.vue";
import Form from "./Form.vue";

const todos = ref<Todo[] | null>(null);
const shownTodos = ref<Todo[] | null>(null);
const sortByPriority = ref(false);
const hideDoneTodos = ref(false);

const remainingTodos = computed(() => {
  return todos.value?.filter((t) => {
    return !t.done;
  }).length;
});

const add = ({
  description,
  priority,
}: {
  description: string;
  priority: number;
}) => {
  if (priority < 1 || priority > 3) return;
  if (description && todos.value) {
    todos.value = [
      ...todos.value,
      {
        id: uuidv4(),
        description,
        priority,
        done: false,
      },
    ];
  }
};

const remove = (id: string) => {
  if (todos.value) {
    todos.value = todos.value.filter((v) => v.id !== id);
  }
};

onMounted(async () => {
  try {
    const { data, status } = await axios.get("http://localhost:4000/todos");
    if (status !== 200) throw new Error("Error");
    todos.value = data;
  } catch (err) {
    todos.value = [
      {
        id: uuidv4(),
        description: "Mocked data, request failed",
        priority: 3,
        done: false,
      },
      {
        id: uuidv4(),
        description: "Call mom",
        priority: 3,
        done: false,
      },
      {
        id: uuidv4(),
        description: "Get Haircut",
        priority: 1,
        done: true,
      },
      {
        id: uuidv4(),
        description: "Call mom again",
        priority: 3,
        done: false,
      },
    ];
    console.error(err);
  }
});

watch(sortByPriority, () => {
  if (shownTodos.value) {
    shownTodos.value = sortByPriority.value
      ? shownTodos.value?.sort((a, b) => b.priority - a.priority)
      : shownTodos.value?.sort((a) => (a.done ? 1 : -1));
  }
});

watchEffect(() => {
  shownTodos.value = hideDoneTodos.value
    ? todos.value?.filter((t) => !t.done) ?? null
    : todos.value;
});

const showPreview = ref(true);
</script>

<template>
  <div class="flex mt-20 ml-40 items-start justify-start gap-4">
    <div>
      <Header :remainingTodos="remainingTodos" />

      <FilterControls
        :sortByPriority="sortByPriority"
        :hideDoneTodos="hideDoneTodos"
        @toggle-done-filter="hideDoneTodos = !hideDoneTodos"
        @toggle-priority-filter="sortByPriority = !sortByPriority"
      />

      <TodoList
        :todos="todos"
        :shownTodos="shownTodos"
        @removeItem="remove($event)"
      />

      <Form @add="add($event)" />
    </div>
    <div>
      <button
        @click="showPreview = !showPreview"
        class="py-1 px-3 mb-2 rounded-xl"
      >
        Toggle preview
      </button>
      <pre v-if="showPreview" class="text-xs">{{
        JSON.stringify(todos, null, 2)
      }}</pre>
    </div>
  </div>
</template>

<style>
button {
  background-color: rgba(209, 213, 219);
  font-weight: 700;
  color: black;
}

button:hover {
  background-color: rgba(156, 163, 175);
}
</style>
