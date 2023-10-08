<template>
  <div>
    <form>
      <input type="text" v-model="newTask.task" placeholder="Enter task" />
      <button @click.prevent="addTask()">Add</button>
    </form>
    <table>
      <tr>
        <th>Status</th>
        <th>Task</th>
        <th>Action</th>
      </tr>
      <tr v-for="todo in todos" :key="todo.id">
        <td>todo</td>
        <td>{{ todo.task }}</td>
        <td>
          <button @click="editTask(todo.id)">Edit</button>
          <button @click="removeTask(todo.id)">Delete</button>
        </td>
      </tr>
    </table>
  </div>
</template>

<style scoped>
th,
td {
  border: 1px solid black;
}

form {
  width: 50%;
  height: 100px;
  display: inline;
}
</style>

<script>
export default {
  data() {
    return {
      newTask: {
        id: "",
        task: "",
      },
      todos: [],
      editing: false,
    };
  },

  beforeMount() {
    let temTodo = localStorage.getItem("new");
    this.todos = JSON.parse(temTodo) || [];
  },
  methods: {
    addTask() {
      if (this.newTask.task !== "") {
        if (this.editing) {
          this.todos = this.todos.map((todo) => {
            if (todo.id == this.newTask.id) {
              return this.newTask;
            }
            return todo;
          });
          localStorage.setItem("new", JSON.stringify(this.todos));
          this.editing = false;
        } else {
          const genId = Math.floor(Math.random() * 1000000);
          this.newTask.id = genId;
          this.todos.push({ ...this.newTask });
          localStorage.setItem("new", JSON.stringify(this.todos));
        }
        this.clearForm();
        this.editing = false;
      }
    },
    editTask(id) {
      this.editing = true;
      let foundTodo = this.todos.find((todo) => {
        if (todo.id == id) return todo;
      });
      this.newTask = {
        id: foundTodo.id,
        task: foundTodo.task,
      };
    },
    removeTask(id) {
      this.todos = this.todos.filter((todo) => {
        return todo.id !== id;
      });
      localStorage.setItem("new", JSON.stringify(this.todos));
    },
    clearForm() {
      this.newTask = {
        id: "",
        task: "",
      };
    },
  },
};
</script>
