<template>
  <div class="container">
    <div class="header">
      <div class="title">TO DO LIST</div>
      <input
        v-model="dataEntry.task"
        type="text"
        class="textbox"
        placeholder="Enter To Do Item..."
      />
      <span class="addBtn" @click.prevent="addData()">Add</span>
    </div>
    <ul class="list-group">
      <li
        class="list-group-item"
        v-for="(item, index) in items"
        :key="index"
        :class="{ disabled: item.done, checked: item.done }"
      >
        <p @click.prevent="doneToDo(item)">{{ item.task }}</p>
        <span @click="editItem(item.id)" class="edit-icon"
          ><font-awesome-icon icon="fa-solid fa-edit" /></span
        ><span @click="removeItem(item.id)" class="delete-icon"
          ><font-awesome-icon icon="fa-solid fa-trash"
        /></span>
      </li>
    </ul>
  </div>
</template>
<script>
export default {
  data() {
    return {
      dataEntry: {
        id: '',
        task: '',
        done: false
      },
      items: [],
      editing: false
    }
  },
  created() {
    this.fetchEntries()
  },
  methods: {
    addData() {
      if (this.dataEntry.task !== '') {
        if (this.editing) {
          this.items = this.items.map((item) => {
            if (item.id == this.dataEntry.id) {
              return this.dataEntry
            }
            return item
          })
          localStorage.setItem('ToDoListItems', JSON.stringify(this.items))
          this.editing = false
        }
        //saving
        else {
          const genId = Math.floor(Math.random() * 1000000)
          this.dataEntry.id = genId
          this.items.push({ ...this.dataEntry })
          localStorage.setItem('ToDoListItems', JSON.stringify(this.items))
        }
        this.clearEntries()
      }
    },
    fetchEntries() {
      const fetched = localStorage.getItem('ToDoListItems')
      this.items = JSON.parse(fetched) || []
    },
    doneToDo(todo) {
      todo.done = !todo.done
      localStorage.setItem('ToDoListItems', JSON.stringify(this.items))
    },
    editItem(id) {
      this.editing = true
      let editData = this.items.find((item) => {
        if (item.id == id) {
          return item
        }
      })
      Object.keys(this.dataEntry).forEach((key) => {
        this.dataEntry[key] = editData[key]
      })
    },
    removeItem(id) {
      this.items = this.items.filter((item) => {
        return item.id !== id
      })
      localStorage.setItem('ToDoListItems', JSON.stringify(this.items))
    },
    clearEntries() {
      this.dataEntry = {
        id: '',
        task: '',
        done: false
      }
    }
  }
}
</script>
