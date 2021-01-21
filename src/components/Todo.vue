<template>
  <div>
    <section class="todoapp">
      <header class="header">
        <h1 id="heading">{{ title }}</h1>
        <input
          class="new-todo"
          placeholder="What needs to be done?"
          v-on:keyup.enter="createTodo"
          autofocus
        />
      </header>
      <Task :list="todos" />
      <section>
        <footer class="footer" v-if="todos.length">
          <span class="todo-count">
            <strong>{{ activeTodos.length }}</strong> item(s) left</span
          >
          <button
            class="clear-completed"
            @click="clearCompleted"
            v-show="completedTodos.length"
          >
            Clear completed
          </button>
        </footer>
      </section>
    </section>
    <footer class="info">
      <p>Click to complete a todo</p>
      <p>Double-click to edit a todo</p>
      <p>Created by Vikrom Narula!</p>
      <p>Part of Front End assignment</p>
    </footer>
  </div>
</template>

<script>
import Task from "./Task.vue";
const LOCAL_STORAGE_KEY = "todo-app-vue";
export default {
  components: {
    Task,
  },
  data() {
    return {
      title: "Change this!",
      todos: JSON.parse(localStorage.getItem(LOCAL_STORAGE_KEY)) || [
        { text: "Learn JavaScript ES6+ goodies", isDone: true },
        { text: "Learn Vue", isDone: false },
        { text: "Build something awesome", isDone: false },
      ],
      editing: null,
    };
  },
  methods: {
    createTodo(event) {
      const textbox = event.target;
      this.todos.push({ text: textbox.value.trim(), isDone: false });
      textbox.value = "";
    },
    clearCompleted() {
      this.todos = this.activeTodos;
    },
  },
  computed: {
    activeTodos() {
      return this.todos.filter((todo) => !todo.isDone);
    },
    completedTodos() {
      return this.todos.filter((todo) => todo.isDone);
    },
  },
  watch: {
    todos: {
      deep: true,
      handler(newValue) {
        localStorage.setItem(LOCAL_STORAGE_KEY, JSON.stringify(newValue));
      },
    },
  },
};
</script>
