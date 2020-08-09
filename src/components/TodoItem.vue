<template>
  <div class="todo-item">
    <div class="todo-item-left">
      <input type="checkbox" v-model="todo.completed" />
      <div
        v-if="!todo.editing"
        class="todo-item-label"
        :class="{ completed: todo.completed }"
        @dblclick="editTodo(todo)"
      >
        {{ todo.title }}
      </div>
      <input
        v-else
        type="text"
        class="todo-item-edit"
        v-model="todo.title"
        @blur="doneTodo(todo)"
        @keyup.enter="doneTodo(todo)"
        @keyup.esc="cancelEdit(todo)"
        v-focus
      />
    </div>

    <div class="remove-item" @click="deleteTodo(index)">
      &times;
    </div>
  </div>
</template>

<script>
export default {
  name: "todo-item",
  props:{
      todo:{
          type:Object,
          required: true
      },
      index:{
          type: Number,
          required: true
      }
  }
};
</script>

<style scoped></style>
