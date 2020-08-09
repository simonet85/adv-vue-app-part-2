<template>
  <div class="todo-item">
    <div class="todo-item-left">
      <input type="checkbox" v-model="completed" @change="doneTodo" />
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
    checkAll: {
      type: Boolean,
      required: true,
    },
  },
  data() {
    //Make a copy of our properties for making them mutatble
    return {
      id: this.todo.id,
      title: this.todo.title,
      editing: this.todo.editing,
      completed: this.todo.completed,
      beforeRditingCache: "",
    };
  },
  directives: {
    focus: {
      inserted: function(el) {
        el.focus();
      },
    },
  },
  methods: {
    deleteTodo: function(index) {
      this.$emit("deletedTodo", index); //emit an event to remove the todo item
    },
    editTodo: function() {
      this.beforeEditCache = this.title; //Caching the title before editing
      this.editing = true;
    },
    doneTodo: function() {
      if (this.title.trim() == "") {
        this.title = this.beforeEditCache;
      }
      this.editing = false;
      this.$emit("finishedEdit", {
        index: this.index,
        todo: {
          id: this.id,
          title: this.title,
          completed: this.completed,
          editing: this.editing,
        },
      });
    },
    cancelEdit: function() {
      this.title = this.beforeEditCache; // Retrieving the cached title while pressing ESC
      this.editing = false;
    },
  },
  watch: {
    //Watch properties when they change and update them accordingly
    checkAll: function() {
      if (this.checkAll) {
        this.completed = true;
      } else {
        this.completed = this.todo.completed;
      }
    },
  },
};
</script>

<style scoped></style>
