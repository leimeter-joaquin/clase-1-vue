<script lang="ts">
import { v4 as uuidv4 } from 'uuid';
export default {
  data() {
    return {
			inputText: '',
      todos: [
        {
          id: uuidv4(),
          description: "Go shoppping",
        },
        {
          id: uuidv4(),
          description: "Call mom",
        },
      ]
    };
  },
	methods: {
		add(description: string) {
      if(description){
        this.todos = [...this.todos, {
          id: uuidv4(),
          description
        }]
        this.inputText = ''
      }
		},
		remove(id: string) {
			this.todos = this.todos.filter((v) => {
				return v.id !== id
			})
		}
	}
};
</script>

<template>
	<div class="w-[300px] m-auto mt-20">
    <h1 class="text-center text-3xl font-bold mb-1"
    >
      Vue 3 To Do List
    </h1>
    <p class="text-center mb-5">
      Remaining ({{ todos.length }})
    </p>
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
        >
        </button>
			</li>
    </ul>
    <div class="flex justify-around mt-5">
      <input v-model="inputText" class="text-black outline-0 pl-2"/>
      <button
        @click="add(inputText)"
        class="rounded-lg px-2 py-1"
      >Add item</button>
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
