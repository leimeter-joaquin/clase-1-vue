<script lang="ts">
export default {
  data() {
    return {
			inputText: '',
      todos: [
        {
          id: Math.floor(Math.random() * 100000).toString(),
          description: "go shoppping",
        },
        {
          id: Math.floor(Math.random() * 100000).toString(),
          description: "call mom",
        },
      ],
    };
  },
	methods: {
		add(description: string) {
			this.todos = [...this.todos, {
				id: Math.floor(Math.random() * 100000).toString(),
				description: description
			}]
		},
		remove(id: string) {
			this.todos = this.todos.filter((v) => {
				return v.id !== id
			})
		}
	},
	watch: {
		todos(v) {
			console.log(v);
		}
	}
};
</script>

<template>
	<div>
		<ul>
      <li v-for="item in todos" :key="item.id">
				{{ item.description }}
				<button @click="remove(item.id)">remove</button>
			</li>
    </ul>
		<input v-model="inputText"/>
		<button @click="add(inputText)">add</button>
  </div>
</template>
