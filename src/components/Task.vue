<template>
  <div>
    <section class="main" v-if="tasks.length">
      <ul class="todo-list">
        <li
          v-for="task in tasks"
          v-bind:key="task.id"
          :class="{ completed: task.isDone, editing: task === editing }"
        >
          <div class="view">
            <input class="toggle" type="checkbox" v-model="task.isDone" />
            <label @dblclick="startEditing(task)">{{ task.text }}</label>
            <button class="destroy" @click="destroyTodo(task)"></button>
          </div>
          <input
            class="edit"
            @keyup.esc="cancelEditing"
            @keyup.enter="finishEditing"
            @blur="finishEditing"
            :value="task.text"
          />
        </li>
      </ul>
    </section>
  </div>
</template>

<script>
export default {
  props: ["tasks"],
  data() {
    return {
      editing: null,
    };
  },
  methods: {
    destroyTodo(task) {
      const index = this.tasks.indexOf(task);
      this.tasks.splice(index, 1);
    },
    startEditing(task) {
      this.editing = task;
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
};
</script>
