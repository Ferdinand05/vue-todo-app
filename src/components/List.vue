<template>
  <ul class="list-group list-group-flush">
    <li class="list-group-item" :class="[todo.isDone ? disabled : '']" v-for="(todo, index) in todos" :key="todo">
      <div class="row">
        <div class="col">
          <span v-if="todo.isDone">
            <del>{{ todo.activity }}</del>
          </span>
          <span v-else>
            {{ todo.activity }}
          </span>
        </div>
        <div class="col">
          <div class="d-flex justify-content-end">
            <button type="button" class="btn btn-info" title="mark done" @click="runDone(index)">&check;</button>
            <button type="button" class="ms-1 btn btn-danger" title="delete" @click="runDelete(index)">X</button>
          </div>
        </div>
      </div>
    </li>
  </ul>
</template>

<script>
import { ref, reactive, toRefs } from "vue";
export default {
  props: {
    todos: {
      type: Array,
      default: [],
    },
  },
  setup(props, { slots, emit, attrs }) {
    const runDelete = (index) => {
      emit("deleteTodo", index);
    };
    const runDone = (index) => {
      emit("doneTodo", index);
    };

    const disabled = "disable";
    return {
      runDelete,
      runDone,
      disabled,
    };
  },
};
</script>

<style>
.disable {
  color: gray;
}
</style>
