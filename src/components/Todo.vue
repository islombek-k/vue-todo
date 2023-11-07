<script setup lang="ts">
import { ref } from "vue";

interface Todo {
  id: number;
  label: string;
}

const input = ref<string | null>(null);
const todos = ref<Todo[]>([]);
let editId = ref<number | null>(null);

const onSubmit = () => {
  if (editId.value) {
    const todoToEdit = todos.value.find((todo) => todo.id === editId.value);
    if (todoToEdit) {
      todoToEdit.label = input.value ?? "";
    }
    editId.value = null;
  } else if (input.value && !editId.value) {
    todos.value.push({ id: todos.value.length + 1, label: input.value });
  }
  input.value = "";
};

const onEditTodo = (id: number) => {
  const todoToEdit = todos.value.find((todo) => todo.id === id);
  if (todoToEdit) {
    input.value = todoToEdit.label;
    editId.value = todoToEdit.id;
  }
};
</script>

<template>
  <form
    class="flex flex-col justify-center items-center mt-12"
    @submit.prevent="onSubmit"
  >
    <div class="flex justify-between w-1/3">
      <input
        placeholder="Write your todo here..."
        type="text"
        v-model="input"
        class="border-gray-500 rounded-m p-2 w-full"
      />
      <button
        class="ml-2 text-white p-2"
        :class="editId ? 'bg-blue-500' : 'bg-green-500'"
      >
        {{ editId ? "Edit" : "Add" }}
      </button>
    </div>
    <ul class="mt-4 w-1/3">
      <li v-for="todo in todos" :key="todo.id" class="mb-4">
        <span class="p-2 text-left bg-white w-[500px] overflow-hidden">
          {{ todo.label }}
        </span>
        <button
          type="button"
          class="text-blue-500 ml-2"
          @click="onEditTodo(todo.id)"
        >
          Edit
        </button>
        <button
          type="button"
          class="text-red-500 ml-2"
          @click="todos.splice(todos.indexOf(todo), 1)"
        >
          Delete
        </button>
      </li>
    </ul>
    <p v-if="!todos.length">No todos here to show</p>
  </form>
</template>
