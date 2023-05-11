<template>
  <ul class="list-group mt-3">
    <transition-group name="bounce">
      <li
        class="list-group-item"
        :class="todo.status ? ['bg-success'] : []"
        v-for="(todo, index) in todos"
        :key="todo">
        <div class="row">
          <div class="col">
            <span v-if="todo.status">
              <del>{{ todo.activity }}</del>
            </span>
            <span v-else>{{ todo.activity }}</span>
          </div>
          <div class="col-auto">
            <button
              class="btn btn-sm mx-2"
              :class="todo.status ? ['btn-warning'] : ['btn-info']"
              @click="changeStatus(index)">
              <span v-if="todo.status"> Undone </span>
              <span v-else> Done </span>
            </button>
            <button class="btn btn-sm btn-danger" @click="deleteTodo(index)">
              X
            </button>
          </div>
        </div>
      </li>
    </transition-group>
  </ul>
</template>

<script>
export default {
  props: {
    todos: {
      type: Array,
      default: [],
    },
  },
  methods: {
    deleteTodo(index) {
      this.$emit("deleteTodo", index);
    },
    changeStatus(index) {
      this.$emit("setTodo", index);
    },
  },
};
</script>
