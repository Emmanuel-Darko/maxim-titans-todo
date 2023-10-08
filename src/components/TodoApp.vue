<template>
  <div class="content">
    <div>
      <h1>My TO-DO</h1>
      <div class="inputField">
        <input class="input" v-model="taskData.items" type="text" placeholder="Enter items" />
        <input type="submit" class="submit-button" @click.prevent="saveData()" :value="btnTitle" />
      </div>
    </div>
    <div class="table-content">
      <h2>Items On The List</h2>
      <table>
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
              <button @click="editItem(item.id)">Edit</button>
              <button @click="removeItem()">Delete</button>
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
.input {
  width: 15rem;
  height: 2rem;
}
.submit-button {
  height: 2.3rem;
  width: 4rem;
  font-weight: bold;
}
h1,
h2 {
  text-align: center;
}

th,
td {
  border: 1px solid black;
  height: 30px;
  padding: 10px;
  text-align: center;
}

tr td button {
  width: 4rem;
  height: 3rem;
  cursor: pointer;
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


