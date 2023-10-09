<template>
  <div class="content">
    <div>
      <h1>My TO-DO</h1>
    </div>

    <div class="table-content">
      <div class="inputField">
        <input class="input" v-model="taskData.items" type="text" placeholder="Enter items" />
        <input type="submit" class="submit-button" @click.prevent="saveData()" :value="btnTitle" />
      </div>
      <h2>Items On The List</h2>
      <table class="table">
        <thead>
          <th>Status</th>
          <th>Task</th>
          <th>Actions</th>
        </thead>
        <tbody>
          <tr v-for="(item, index) in listItem" :key="index">
            <td>
              <input class="tick" type="checkbox" />
            </td>
            <td>{{ item.items }}</td>
            <td>
              <button class="edit-btn" @click="editItem(item.id)">Edit</button>
              <button class="delete-btn" @click="removeItem()">Delete</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

// Styling

<style scoped>
.content {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.inputField {
  margin: 0;
  padding-top: 10px;
}
.input {
  width: 15rem;
  height: 2rem;
}
.submit-button {
  height: 2.3rem;
  width: 4rem;
  font-weight: bold;
  background-color: black;
  color: white;
  margin-left: 5px;
}
h1,
h2 {
  text-align: center;
}
h2 {
  margin-top: 30px;
  padding: 0;
}

.table-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 15px 5px;
}

.table {
  margin-bottom: 20px;
}

th,
td {
  border: 1px solid black;
  height: 25px;
  padding: 10px;
  text-align: center;
}

tr td button {
  width: 4rem;
  height: 2rem;
  cursor: pointer;
  border-radius: 10px;
}

.edit-btn {
  margin-right: 5px;
  background-color: black;
  color: white;
}
.delete-btn {
  background-color: white;
}
</style>

<script>
export default {
  data() {
    return {
      taskData: {
        items: '',
        id: ''
        // itemStatus: ['to-do', 'in progress', 'done'],
        // task: ''
      },

      listItem: [],
      editingData: false
    }
  },

  beforeMount() {
    this.getData()
  },
  methods: {
    saveData() {
      if (this.taskData.items !== '') {
        if (this.editingData) {
          this.listItem = this.listItem.map((item) => {
            if (item.id == this.taskData.id) {
              return this.taskData
            }
            return item
          })
          localStorage.setItem('item', JSON.stringify(this.listItem))
          this.editingData = false
        } else {
          const genId = Math.floor(Math.random() * 1000000)
          this.taskData.id = genId
          this.listItem.push({ ...this.taskData })
          localStorage.setItem('item', JSON.stringify(this.listItem))
        }
        this.clearData()
      }
    },

    getData() {
      let tempData = localStorage.getItem('item')
      this.listItem = JSON.parse(tempData) || []
    },

    editItem(id) {
      this.editingData = true
      let itemSelected = this.listItem.find((item) => {
        if (item.id == id) {
          return item
        }
      })
      this.taskData = {
        items: itemSelected.items,
        id: itemSelected.id
      }
    },

    removeItem(index) {
      this.listItem.splice(index, 1)
      localStorage.setItem('item', JSON.stringify(this.listItem))
    },

    clearData() {
      this.editingData = false
      this.taskData = {
        id: '',
        items: ''
      }
    }
    // changeStatus(index){
    //   this.itemStatus.indexOf()
    // }
  }
}
</script>


