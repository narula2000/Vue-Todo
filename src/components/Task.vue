<template>
  <div>
    <section class="main" v-if="list.length">
      <ul class="todo-list">
        <li
          v-for="todo in list"
          v-bind:key="todo.id"
          :class="{ completed: todo.isDone, editing: todo === editing }"
        >
          <div class="view">
            <input class="toggle" type="checkbox" v-model="todo.isDone" />
            <label @dblclick="startEditing(todo)">{{ todo.text }}</label>
            <button class="destroy" @click="destroyTodo(todo)"></button>
          </div>
          <input
            class="edit"
            @keyup.esc="cancelEditing"
            @keyup.enter="finishEditing"
            @blur="finishEditing"
            :value="todo.text"
          />
        </li>
      </ul>
    </section>
  </div>
</template>

<script>
const LOCAL_STORAGE_KEY = "todo-app-vue";
export default {
  props: ["list"],
  data() {
    return {
      editing: null,
    };
  },
  methods: {
    destroyTodo(todo) {
      const index = this.list.indexOf(todo);
      this.list.splice(index, 1);
    },
    startEditing(todo) {
      this.editing = todo;
    },
    finishEditing(event) {
      if (!this.editing) {
        return;
      }
      const textbox = event.target;
      this.editing.text = textbox.value.trim();
      this.editing = null;
    },
    cancelEditing() {
      this.editing = null;
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
