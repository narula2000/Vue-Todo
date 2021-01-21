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
      <tasks :tasks="todos" />
      <footer class="footer" v-if="todos.length">
        <span class="todo-count">
          <strong>{{ activeTodos.length }}</strong>
          item(s) left
        </span>
        <button
          class="clear-completed"
          @click="clearCompleted"
          v-show="completedTodos.length"
        >
          Clear completed
        </button>
      </footer>
    </section>
    <footnote />
  </div>
</template>

<script>
import Tasks from "./Task.vue";
import Footnote from "./Footnote.vue";

const LOCAL_STORAGE_KEY = "todo-app-vue";

export default {
  components: {
    Tasks,
    Footnote,
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
