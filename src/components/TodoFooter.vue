<template>
    <footer class="footer">
        <span class="todo-count">
          <strong>{{ activeTodoCount }}</strong>
          item(s) left
        </span>
        <button
          class="clear-completed"
          @click="$emit('todo-action-clear')"
          v-show="compeleteTodoCount"
        >
          Clear completed
        </button>
      </footer>
</template>

<script>
export default {
    props: {
        activeTodoCount: {
            type: Number,
            required: true
        },
        compeleteTodoCount: {
            type: Number,
            required: true
        }
    }
}
</script>