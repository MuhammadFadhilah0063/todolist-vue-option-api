<template>
  <div class="container mt-2">
    <div class="card">
      <div class="card-body">
        <div class="card-title">
          <h1>Simple Todolist</h1>
        </div>
        <div class="row">
          <div class="col-10">
            <input
              type="text"
              class="form-control"
              v-model="todo"
              @keyup.enter="add" />
          </div>
          <div class="col-2">
            <button class="btn btn-success" @click="add">ADD</button>
          </div>
        </div>
        <h5 class="mt-3">Todo: {{ count }}</h5>
        <transition-group name="bounce">
          <List
            v-show="todos"
            v-if="todos.length != 0"
            :todos="todos"
            @deleteTodo="deleteTodo"
            @setTodo="setTodo" />
          <h1 v-else>Loading...</h1>
        </transition-group>
      </div>
    </div>
  </div>
</template>

<script>
import List from "./components/List.vue";

export default {
  components: {
    List,
  },
  data() {
    return {
      todo: "",
      todos: [],
    };
  },
  computed: {
    count() {
      return this.todos.length;
    },
  },
  methods: {
    add() {
      this.todos.unshift({
        activity: this.todo,
        status: false,
      });
      this.todo = "";

      this.saveToLocalStorage();
    },
    deleteTodo(todoIndex) {
      this.todos = this.todos.filter((todo, index) => {
        if (todoIndex != index) {
          return todo;
        }
      });

      this.saveToLocalStorage();
    },
    setTodo(todoIndex) {
      this.todos = this.todos.filter((todo, index) => {
        if (todoIndex == index) {
          todo.status ? (todo.status = false) : (todo.status = true);
        }

        return todo;
      });

      this.saveToLocalStorage();
    },
    saveToLocalStorage() {
      localStorage.setItem("todos", JSON.stringify(this.todos));
    },
  },
  mounted() {
    setTimeout(() => {
      this.todos = JSON.parse(localStorage.getItem("todos"));
    }, 2000);
  },
};
</script>

<style>
.bounce-enter-active {
  animation: bounce-in 0.5s;
}
.bounce-leave-active {
  animation: bounce-in 0.5s reverse;
}
@keyframes bounce-in {
  0% {
    transform: scale(0);
  }
  50% {
    transform: scale(1.25);
  }
  100% {
    transform: scale(1);
  }
}

.slide-enter-active,
.slide-leave-active {
  transition: transform 0.5s ease-out;
}

.slide-enter,
.slide-leave-to {
  transform: translateX(100%);
}
</style>
