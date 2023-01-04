<script setup lang="ts">
import { v4 as uuidv4 } from "uuid";
import { computed, onMounted, ref, watch } from "vue";
import axios from "axios";
import { Todos } from "../types";
import Header from "./Header.vue";
import FilterControls from "./FilterControls.vue";
import List from "./List.vue";
import Form from "./Form.vue";

const todos = ref<Todos[] | null>(null);
const shownTodos = computed(() => {
  if (hideDoneTodos.value) return todos.value?.filter((t) => !t.done);
  return todos.value;
});

const hideDoneTodos = ref(false);

const remainingTodos = computed(() => {
  return todos.value?.filter((t) => {
    return !t.done;
  }).length;
});

const sortByPriority = ref(false);

watch(sortByPriority, () => {
  if (sortByPriority.value) {
    shownTodos.value?.sort((a, b) => b.priority - a.priority);
  } else {
    shownTodos.value?.sort((a) => (a.done ? 1 : -1));
  }
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
    todos.value = todos.value.filter((v) => {
      return v.id !== id;
    });
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

const showPreview = ref(true);
</script>

<template>
  <div class="flex mt-20 ml-40 items-start justify-start gap-4">
    <div>
      <Header :remaining-todos="remainingTodos" />

      <FilterControls
        v-model:hideDoneTodos="hideDoneTodos"
        v-model:sortByPriority="sortByPriority"
      />

      <List
        :todos="todos"
        :hide-done-todos="hideDoneTodos"
        @remove="remove($event)"
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
