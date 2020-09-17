<template>
  <div>
    <div class="input-group">
      <input type="text" class="form-control" placeholder="enter your task" ref="inputTask" />
      <button class="btn btn-secondary" type="button" @click="addTask">Add task</button>
    </div>
    <b-list-group ref="listOfTasks" class="taskList">
      <template v-if="show">
        <b-list-group-item class="d-flex flex-wrap" v-for="(todo,index) in falseList" :key="index">
          <h6 style="width: 80%; color: green">
            {{index +1}}.
            {{todo.text}}
          </h6>
          <button @click="markedTrue(todo.id)">
            <b-icon icon="check2-square" scale="1.5" variant="success"></b-icon>
          </button>
          <button @click="markedFalse(todo.id)">
            <b-icon icon="x-circle" scale="1.5" variant="primary"></b-icon>
          </button>
          <button @click="deleteTask(todo.id)">
            <b-icon icon="trash" scale="1.5" variant="danger"></b-icon>
          </button>
        </b-list-group-item>
      </template>
      <template v-else>
        <b-list-group-item class="d-flex flex-wrap" v-for="(todo,index) in trueList" :key="index">
          <h6 style="width: 80%; color: red">{{index+1}}. {{todo.text}}</h6>
          <button @click="markedTrue(todo.id)">
            <b-icon icon="check2-square" scale="1.5" variant="primary"></b-icon>
          </button>
          <button @click="markedFalse(todo.id)">
            <b-icon icon="x-circle" scale="1.5" variant="warning"></b-icon>
          </button>
          <button @click="deleteTask(todo.id)">
            <b-icon icon="trash" scale="1.5" variant="danger"></b-icon>
          </button>
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
    trueList: function () {
      return this.todos.filter((e) => !e.completed);
    },
    falseList: function () {
      return this.todos.filter((e) => e.completed);
    },
  },
  methods: {
    addTask() {
      this.todosText = this.$refs.inputTask.value;
      this.todos.push({
        text: this.todosText,
        completed: false,
        id: Math.random(),
      });
      this.todosText = "";
      this.$refs.inputTask.value = "";
    },
    deleteTask(todo) {
            for(let i =0;i<this.todos.length;i++) {
        if(todo == this.todos[i].id){
          this.todos.splice(i, 1);
        }
      }
    },
    ShowNotCompleted() {
      this.show = false;
    },
    showCompleted() {
      this.show = true;
    },
    markedTrue(todo) {
   this.todos.forEach((el) => {
        if (todo == el.id) {
          el.completed = true;
        }
      });
    },
    markedFalse(todo) {
      this.todos.forEach((el) => {
        if (todo == el.id) {
          el.completed = false;
        }
      });
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
button {
  height: 38px;
}
</style>
