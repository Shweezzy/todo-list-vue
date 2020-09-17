<template>
  <div>
    <div class="input-group">
      <input type="text" class="form-control" placeholder="enter your task" ref="inputTask" />
      <button class="btn btn-secondary" type="button" @click="addTask">Add task</button>
    </div>
    <b-list-group ref="listOfTasks" class="taskList">
      <template v-if="show">
        <b-list-group-item class="d-flex flex-wrap" v-for="(todo,index) in false1" :key="index">
          <h6 style="width: 85%; color: green">
            {{index +1}}.
            {{todo.text}}
          </h6>
          <b-icon @click="markedTrue(index)" icon="check2-square" scale="1.5" variant="success"></b-icon>
          <b-icon @click="markedFalse(index)" icon="x-circle" scale="1.5" variant="primary"></b-icon>
          <b-icon @click="deleteTask(index)" icon="trash" scale="1.5" variant="danger"></b-icon>
        </b-list-group-item>
      </template>
      <template v-else>
        <b-list-group-item class="d-flex flex-wrap" v-for="(todo,index) in true2" :key="index">
          <h6 style="width: 85%; color: red">{{index+1}}. {{todo.text}}</h6>
          <b-icon id="asd" @click="markedTrue(index)" icon="check2-square" scale="1.5" variant="primary"></b-icon>
          <b-icon @click="markedFalse(index)" icon="x-circle" scale="1.5" variant="warning"></b-icon>
          <b-icon @click="deleteTask(index)" icon="trash" scale="1.5" variant="danger"></b-icon>
        </b-list-group-item>
      </template>
    </b-list-group>
    <div class="filterButtons">
      <button @click="showCompleted" type="button" class="btn">Completed</button>
      <button @click="ShowNotCompleted" type="button" class="btn">Not completed</button>
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
    show: false,
  }),
  computed: {
    true2: function () {
      return this.todos.filter((e) => !e.completed);
    },
    false1: function () {
      return this.todos.filter((e) => e.completed);
    },
  },
  methods: {
    addTask() {
      this.todosText = this.$refs.inputTask.value;
      this.todos.push({ text: this.todosText, completed: false });
      this.todosText = "";
      this.$refs.inputTask.value = "";
    },
    deleteTask(index) {
      this.todos.splice(index, 1);
    },
    ShowNotCompleted() {
      this.show = false;
    },
    showCompleted() {
      this.show = true;
    },
    markedTrue(index) {
      this.todos[index].completed = true;
    },
    markedFalse(index) {
      this.todos[index].completed = false;
    },
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
.filterButtons {
  text-align: center;
}
.btn-light {
  margin: 1px;
}
.b-icon.bi {
  width: 2em;
}
</style>
