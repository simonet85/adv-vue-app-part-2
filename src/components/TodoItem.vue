<template>
  <div class="todo-item">
    <div class="todo-item-left">
      <input type="checkbox" v-model="completed" />
      <div
        v-if="!editing"
        class="todo-item-label"
        :class="{ completed: completed }"
        @dblclick="editTodo"
      >
        {{ title }}
      </div>
      <input
        v-else
        type="text"
        class="todo-item-edit"
        v-model="title"
        @blur="doneTodo"
        @keyup.enter="doneTodo"
        @keyup.esc="cancelEdit"
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
  props: {
    todo: {
      type: Object,
      required: true,
    },
    index: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      id: this.todo.id,
      title: this.todo.title,
      completed: this.todo.editing,
      beforeRditingCache: "",
    };
  },
  methods: {
    deleteTodo(index) {
      this.$emit("deletedTodo", index); //emit an event to remove the todo item
    },
  },
};
</script>

<style scoped></style>
