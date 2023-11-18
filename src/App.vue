<template>
  <div class="container m-auto mt-3">
    <div class="card">
      <div class="card-header text-center">
        <h2>TODO APP</h2>
      </div>
      <div class="card-body">
        <div class="row">
          <div class="col">
            <div class="input-group mb-3">
              <input type="text" class="form-control" placeholder="whats on your mind?" v-model="todo" @keyup.enter="add" />
              <button class="btn btn-primary" type="button" id="button-addon2" @click="add">Add</button>
            </div>
          </div>
        </div>
        <div class="row mt-3">
          <div class="col">
            <List :todos="todos.list" @deleteTodo="deleteTodo" @doneTodo="doneTodo" />
          </div>
        </div>
        <div class="row text-center mt-3">
          <div class="col">
            <hr class="border-2 border-primary" />
            <small>{{ totalTodo }} Todo</small>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import { ref, reactive, onMounted, computed, onBeforeMount } from "vue";
import List from "./components/List.vue";
export default {
  components: { List },
  setup() {
    const todo = ref("");
    const todos = reactive({
      list: [],
    });

    onMounted(() => {
      const items = localStorage.getItem("todos");
      todos.list = items ? JSON.parse(items) : [];
    });

    const totalTodo = computed(() => {
      return todos.list.length;
    });

    const add = () => {
      todos.list.unshift({
        activity: todo.value,
        isDone: false,
      });
      todo.value = "";
      saveToLocalStorage();
    };

    const deleteTodo = (todoIndex) => {
      todos.list = todos.list.filter((item, index) => {
        if (index != todoIndex) {
          return item;
        }
      });
      saveToLocalStorage();
    };

    const doneTodo = (todoIndex) => {
      todos.list = todos.list.filter((item, index) => {
        if (index == todoIndex) {
          item.isDone = !item.isDone;
        }
        return item;
      });
      saveToLocalStorage();
    };

    const saveToLocalStorage = () => {
      localStorage.setItem("todos", JSON.stringify(todos.list));
    };

    return {
      todo,
      todos,
      totalTodo,
      add,
      deleteTodo,
      doneTodo,
    };
  },
};
</script>

<style></style>
