<template>
  <div id="content">
        <div>
            <h1>Easy-Todo App</h1>

            <h5>Making the most out of your day?</h5>
            <br>
            <hr>

            <div class="table">
            <h2> {{newToDo}} </h2>
            <input  type="text" v-model="dataEntry.task" >
            <button class="addBtn" @click.prevent="createData()" 
              v-bind:disabled= "dataEntry.task.length<2"> +Add</button>
            </div>

            <table>
            <tr>
              <th>Status</th>
              <th>Input</th>
              <th>Toggle</th>
            </tr>
            <tr v-for="item in todos" :key="item.id">
                <td><input type="checkbox" @click="checked(item)"></td>
                <td>{{ item.task }}</td>
                <td>
                    <button class="editBtn" @click="editFn(item.id)">Edit</button>
                    <button class="deleteBtn" @click="deleteFn(item.id)">Delete</button>
                </td>
            </tr>
            </table>

            <br>
            <hr>
        </div>

        <div><p> &copy;2023 </p></div>
  </div>
</template>

<script>
export default{ 
  data(){
    return{
      newToDo:"Let's start planning",
      dataEntry: {
        id: '',
        task:'',
        completed: false
        
      },
      todos: [],
      editing: false
    }
  }, 

  beforeMount() {
    let apTodo = localStorage.getItem('itemAll');
    this.todos =JSON.parse(apTodo) || []
  },
  
  methods: {
    createData(){
      if (this.editing){
        this.todos = this.todos.map((todo)=>{
        if (todo.id== this.dataEntry.id) {
          return this.dataEntry
        } 
        return todo
      })
      localStorage.setItem('itemAll', JSON.stringify(this.todos))
      this.editing = false
    }
      // this part be the saving part
    else{
      const genId = Math.floor(Math.random() * 1000000)
      this.dataEntry.id = genId
      this.todos.push({...this.dataEntry})
      localStorage.setItem('itemAll',JSON.stringify(this.todos))
      }
      this.dataEntry= {
        id: '',
        task:''}
    },

    editFn(id){
      this.editing = true
      let editData = this.todos.find((todo) => {
        if (todo.id ==id) {
          return todo
      }
      })

    Object.keys(this.dataEntry).forEach((key) => {
      this.dataEntry[key] = editData[key]
    })
    },
    
    deleteFn(id){
      this.todos = this.todos.filter((todo) =>{
        return todo.id !==id
      })
      localStorage.setItem('itemAll',JSON.stringify(this.todos))
    }
  },
  checked(todo){
    todo.completed= !todo.completed
    localStorage.setItem('itemAll',JSON.stringify(this.todos))
  }
}

</script>