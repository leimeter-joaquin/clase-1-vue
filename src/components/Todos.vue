<script lang="ts">
import { v4 as uuidv4 } from "uuid";
import { ref } from "vue";
export default {
  setup() {
    const todos = ref([
      {
        id: uuidv4(),
        description: "Go shoppping",
      },
      {
        id: uuidv4(),
        description: "Call mom",
      },
    ]);

    const inputText = ref("");

    const add = (description: string) => {
      if (description) {
        todos.value = [
          ...todos.value,
          {
            id: uuidv4(),
            description,
          },
        ];
        inputText.value = "";
      }
    };

    const remove = (id: string) => {
      todos.value = todos.value.filter((v) => {
        return v.id !== id;
      });
    };

    return {
      todos,
      inputText,
      add,
      remove,
    };
  },
};
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
      >
        <p class="ml-2">{{ item.description }}</p>
        <button
          @click="remove(item.id)"
          class="rounded-full w-5 h-5 ml-5"
        ></button>
      </li>
    </ul>
    <div class="flex justify-around mt-5">
      <input v-model="inputText" class="text-black outline-0 pl-2" />
      <button @click="add(inputText)" class="rounded-lg px-2 py-1">
        Add item
      </button>
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
