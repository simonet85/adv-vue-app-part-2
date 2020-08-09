<template>
  <div>
    <input
      type="text"
      name=""
      class="todo-input"
      placeholder="what need to be done!"
      v-model="newTodo"
      @keyup.enter="createTodo"
    />
    <transition-group
      name="fade"
      enter-active-class="animated fadeInUp"
      leave-active-class="animated fadeOutDown"
    >
      <div
        v-for="(todo, index) in todosFiltered"
        :key="todo.id"
        class="todo-item"
      >
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
    </transition-group>
    <div class="extra-container">
      <div>
        <label for="">
          <input
            type="checkbox"
            :checked="!anyRemaining"
            @change="checkAllTodos"
          />
          Check All
        </label>
      </div>
      <div>{{ remaining }} items left</div>
    </div>
    <div class="extra-container">
      <div>
        <button :class="{ active: filter == 'all' }" @click="filter = 'all'">
          All
        </button>
        <button
          :class="{ active: filter == 'active' }"
          @click="filter = 'active'"
        >
          Active
        </button>
        <button
          :class="{ active: filter == 'completed' }"
          @click="filter = 'completed'"
        >
          completed
        </button>
      </div>
      <div>
        <transition name="fade">
          <button v-if="showClearCompletedButton" @click="clearCompleted">
            Clear Completed
          </button>
        </transition>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      beforeEditCache: "",
      newTodo: "",
      newId: 3,
      filter: "all",
      todos: [
        {
          id: 1,
          title: "Finish todo",
          completed: false,
          editing: false,
        },
        {
          id: 2,
          title: "To take over ",
          completed: false,
          editing: false,
        },
      ],
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
    createTodo: function() {
      if (this.newTodo.trim().length === 0) {
        return;
      }
      this.todos.push({
        id: this.newId,
        title: this.newTodo,
        completed: false,
      });
      this.newTodo = "";
      this.newId++;
    },
    deleteTodo: function(index) {
      this.todos.splice(index, 1);
    },
    editTodo: function(todo) {
      this.beforeEditCache = todo.title; //Caching the title before editing
      todo.editing = true;
    },
    doneTodo: function(todo) {
      if (todo.title.trim() == "") {
        todo.title = this.beforeEditCache;
      }
      todo.editing = false;
    },
    cancelEdit: function(todo) {
      todo.title = this.beforeEditCache; // Retrieving the cached title while pressing ESC
      todo.editing = false;
    },
    checkAllTodos: function() {
      this.todos.forEach((todo) => {
        todo.completed = event.target.checked;
      });
    },
    clearCompleted: function() {
      this.todos = this.todos.filter((todo) => !todo.completed);
    },
  },
  computed: {
    remaining: function() {
      return this.todos.filter((todo) => !todo.completed).length;
    },
    anyRemaining: function() {
      return this.remaining !== 0;
    },
    todosFiltered: function() {
      if (this.filter == "all") {
        return this.todos;
      } else if (this.filter == "active") {
        return this.todos.filter((todo) => !todo.completed);
      } else if (this.filter == "completed") {
        return this.todos.filter((todo) => todo.completed);
      } else {
        return this.todos;
      }
    },
    showClearCompletedButton: function() {
      return this.todos.filter((todo) => todo.completed).length > 0;
    },
  },
};
</script>

<style lang="scss">
@import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.5.2/animate.min.css");
.todo-input {
  width: 100%;
  padding: 10px 18px;
  font-size: 18px;
  margin-bottom: 16px;
  &:focus {
    outline: 0;
  }
}
.todo-item {
  margin-bottom: 12px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  animation-duration: 0.3s;
}
.remove-item {
  cursor: pointer;
  margin-left: 14px;
  &:hover {
    color: tomato;
  }
}
.todo-item-left {
  display: flex;
  align-items: center;
}
.todo-item-label {
  padding: 10px;
  border: 1px solid #fff;
  margin-left: 12px;
}
.todo-item-edit {
  font-size: 24px;
  color: #2c3e50;
  margin-left: 12px;
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  &:focus {
    outline: none;
  }
}
.completed {
  text-decoration: line-through;
  color: #8f9092;
}
.extra-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 16px;
  border-top: 14px;
  margin-bottom: 14px;
}
button {
  font-size: 14px;
  background-color: #fff;
  appearance: none;
  &:hover {
    background: lightgreen;
  }
  &:focus {
    outline: none;
  }
}
.active {
  background: lightgreen;
}
/**transition */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.2s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
/**transition ends */
</style>
