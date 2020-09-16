<template>
  <div>
    <div class="input-group">
      <input type="text" class="form-control" placeholder="Search this blog" ref="inputTask" />
      <button class="btn btn-secondary" type="button" @click="addTask">Add task</button>
    </div>
    <b-list-group class="taskList">
      <b-list-group-item v-for="(todo,index) in todos" :key="index">
        {{todo}}
        <button @click="marked">completed</button>
        <button @click="deleteTask(index)">delete</button>
      </b-list-group-item>
    </b-list-group>
    <div class="filterButtons">
      <button>Show all</button>
      <button @click="test">Completed</button>
      <button>Not completed</button>
    </div>
  </div>
</template>

<script>
import "bootstrap/dist/css/bootstrap.css";
import "bootstrap-vue/dist/bootstrap-vue.css";

export default {
  name: "HelloWorld",
  data: () => ({
    todosText: "",
    todos: [],
  }),
  methods: {
    addTask() {
      this.todosText = this.$refs.inputTask.value;
      this.todos.push(this.todosText);
      this.todosText = "";
      this.$refs.inputTask.value = "";
    },
    deleteTask(index) {
      this.todos.splice(index, 1);
    },
    test() {
      console.log("a");
    },
    marked(e) {
      console.log(e)
    }
  },
  mounted() {
    if (localStorage.getItem("todos"))
      this.todos = JSON.parse(localStorage.getItem("todos"));
  },
  watch: {
    todos: {
      handler() {
        localStorage.setItem(`todos`, JSON.stringify(this.todos));
      },
      deep: true,
    },
  },
};
</script>

<style scoped>
</style>
