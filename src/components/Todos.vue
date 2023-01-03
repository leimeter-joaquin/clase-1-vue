<script setup lang="ts">
import { v4 as uuidv4 } from "uuid";
import { Switch } from "@headlessui/vue";
import { computed, onMounted, ref, watch, watchEffect } from "vue";
import { XMarkIcon } from "@heroicons/vue/24/solid";
import axios from "axios";

interface Todos {
  id: string;
  description: string;
  priority: number;
  done: boolean;
}

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

const sortByPiority = ref(false);

watch(sortByPiority, () => {
  if (sortByPiority.value) {
    shownTodos.value?.sort((a, b) => b.priority - a.priority);
  } else {
    shownTodos.value?.sort((a) => (a.done ? 1 : -1));
  }
});

const sortingBy = computed(() => {
  return `Sorting by ${sortByPiority.value ? "priority" : "status"}`;
});

const textInput = ref("");
const priorityInput = ref(1);

const add = (description: string, priority: number) => {
  if (priorityInput.value < 1 || priorityInput.value > 3) return;
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
    textInput.value = "";
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
    <div class="bg-[#333] p-2 rounded">
      <div class="text-center flex flex-col mb-5 gap-1">
        <h1 class="text-3xl font-bold">Vue 3 To Do List</h1>
        <p class="">Remaining ({{ remainingTodos }})</p>
      </div>

      <div class="flex justify-end p-2 mb-3 rounded bg-[#35485e] gap-4">
        <button
          class="py-1 px-3 button"
          @click="hideDoneTodos = !hideDoneTodos"
        >
          {{ !hideDoneTodos ? "Hide" : "Show" }} done todos
        </button>
        <p class="text-right">Sort By</p>
        <div class="flex gap-4">
          <span>status</span>
          <Switch
            v-model="sortByPiority"
            class="relative inline-flex h-6 w-11 items-center rounded-full bg-white"
          >
            <span class="sr-only">{{ sortingBy }}</span>
            <span
              :class="sortByPiority ? 'translate-x-6' : 'translate-x-1'"
              class="inline-block h-4 w-4 transform rounded-full bg-[#242424] transition"
            />
          </Switch>
          <span>priority</span>
        </div>
      </div>

      <ul v-auto-animate class="space-y-2 flex flex-col items-end" v-if="todos">
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
            @click="remove(item.id)"
            class="rounded-full w-5 h-5 flex items-center justify-center"
          >
            <XMarkIcon class="h-4" />
          </button>
        </li>
      </ul>
      <span v-else>...Loading</span>

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

    <div>
      <button @click="showPreview = !showPreview" class="py-1 px-3 mb-2 button">
        toggle preview
      </button>
      <pre v-if="showPreview" class="text-xs">{{
        JSON.stringify(todos, null, 2)
      }}</pre>
    </div>
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
