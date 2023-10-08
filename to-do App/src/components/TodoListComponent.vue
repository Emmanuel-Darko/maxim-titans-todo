<template>
  <div>
    <div class="inputts">
      <input type="text" v-model="newTask.task" placeholder="Enter task" />
      <button class="addBtn" @click.prevent="addTask()">Add</button>
    </div>

    <br /><br />

    <table>
      <tr>
        <th>Status</th>
        <th>Task</th>
        <th>Action</th>
      </tr>
      <tr v-for="todo in todos" :key="todo.id">
        <td><input type="checkbox" /></td>
        <td>{{ todo.task }}</td>
        <td>
          <button class="editBtn" @click="editTask(todo.id)">Edit</button>
          <button class="deleteBtn" @click="removeTask(todo.id)">Delete</button>
        </td>
      </tr>
    </table>

    <br />
    <br />
    <hr />
    <p>&copy;2023</p>
  </div>
</template>

<style>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

p {
  text-align: center;
}
.inputts {
  display: flex;
  justify-content: center;
}

input {
  width: 80%;
  height: 2rem;
  border-radius: 20px;
  padding-left: 10px;
  border: none;
  background-color: rgb(0, 0, 0);
  color: white;
}

.addBtn {
  border: 2px solid rgb(0, 0, 0);
  border-radius: 25px;
  height: 2rem;
  width: 20%;
}

.editBtn {
  border: none;
  border-radius: 25px;
  height: 1.5rem;
  width: 90px;
  background-color: rgb(23, 93, 168);
  color: white;
  margin-bottom: 5px;
}

.deleteBtn {
  border: none;
  border-radius: 25px;
  height: 1.5rem;
  width: 90px;
  background-color: red;
  color: white;
}

table {
  width: 100%;
  height: 100%;
  border-radius: 20px;
  text-align: center;
  padding: 5px;
}
th,
td {
  border: 2px solid rgb(214, 210, 210);
  padding: 10px;
}

th {
  border-bottom: solid 10px black;
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
