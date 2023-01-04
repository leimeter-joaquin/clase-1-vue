<script setup lang="ts">
import { ref } from "vue";
import { Todo } from "../types";
import Modal from "./Modal.vue";
import TodoItem from "./TodoItem.vue";

defineProps<{
  todos?: Todo[] | null;
  shownTodos?: Todo[] | null;
}>();

const emit = defineEmits<{
  (e: "removeItem", id: string): void;
}>();

const showModal = ref(false);
const selectedTodo = ref<Todo>();

const handleopenModal = (todo: Todo) => {
  showModal.value = true;
  selectedTodo.value = todo;
};

const handleItemRemoval = () => {
  if (selectedTodo.value) {
    emit("removeItem", selectedTodo.value.id);
  }
  showModal.value = false;
};
</script>

<template>
  <Modal @close="showModal = false" :show="showModal">
    <div>
      <div class="py-8 px-8 text-black">
        <p class="mb-8">Are you sure you want to delete this todo?</p>
        <p class="font-bold text-center">{{ selectedTodo?.description }}</p>
      </div>
      <div class="flex p-4 justify-center gap-8">
        <button class="px-6 py-2" @click="showModal = false">close</button>
        <button
          v-if="selectedTodo"
          class="px-6 py-2"
          @click="handleItemRemoval()"
        >
          Delete
        </button>
      </div>
    </div>
  </Modal>

  <ul
    v-auto-animate
    class="space-y-2 flex flex-col items-end py-4"
    v-if="todos"
  >
    <TodoItem
      v-for="item in shownTodos"
      :key="item.id"
      :todoItem="item"
      @removeItem="handleopenModal"
    />
  </ul>
  <span v-else>...Loading</span>
</template>
<style scoped></style>
