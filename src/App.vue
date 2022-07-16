<script setup>
import { reactive, ref } from "vue";
import TheHeader from "./components/TheHeader.vue";
import Todo from "./models/Todo";
import TodoList from "./components/TodoList.vue";

const name = ref("");
const todos = reactive([]);

const addTodo = () => {
  const val = name.value;
  if (!val) return;

  name.value = "";
  todos.push(new Todo(val));
};

const switchTodoStatusByIndex = (index) => {
  todos[index].done = !todos[index].done;
};

const deleteTodoByIndexes = (...indexes) => {
  const newTodos = todos.filter((_, i) => !indexes.includes(i));
  todos.splice(0, todos.length, ...newTodos);
};
</script>

<template>
  <TheHeader />

  <main class="flex flex-col items-center">
    <form @submit.prevent="addTodo">
      <div class="flex items-center justify-center">
        <input
          id="todo-input"
          class="border-b outline-none w-96 px-4 py-2"
          type="text"
          v-model="name"
          placeholder="Please input todo name..."
        />
        <button class="bg-blue-500 text-white px-4 py-2">Add</button>
      </div>
    </form>

    <TodoList
      :todos="todos"
      @deleteByIndexes="deleteTodoByIndexes"
      @switchStatusByIndex="switchTodoStatusByIndex"
    />
  </main>
</template>
