<template>
  <div id="app">
    <h1>todos</h1>
    <div>
      <div class="container">
        <header>
          <button @click="checkAll" class="btn btn-info">Check all</button>
          <button @click="UnCheckAll" class="btn btn-warning">UnCheck all</button>
          <input v-model="task" @keypress.enter="addTodo" type="text">
        </header>
        <body>
        <div>
          <ul>
            <li v-for="task in getTasks" v-bind:key="task.id">
              <input v-model="task.completed" type="checkbox">
              <del v-if="task.completed == true">{{ task.task }} {{task.completed}}</del>
              <label v-else="">{{ task.task }}</label>
              <button @click="deleteTodo(task.id)" class="btn btn-danger">remove</button>
            </li>
          </ul>

        </div>
        </body>
        <footer>
          <div>
            <label>{{ tasks.length }} item left</label>
            <button value="all" @click="isAll" class="btn btn-info">All</button>
            <button id="activeButton" value="active"  @click="isActive" class="btn btn-info">Active {{ activeTasks.length }}</button>
            <button value="completed" @click="isCompleted" class="btn btn-info">Completed {{ completedTasks.length }}</button>
          </div>
          <button @click="deleteCompleted(task.id)" v-if="completedTasks.length > 0" class="btn btn-danger">Delete completed</button>
        </footer>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      id: 0,
      task: '',
      completed: false,
      tasks: []
    }
  },
  methods: {
    addTodo() {
      if(this.task.length > 0){
        this.tasks.push({
          task: this.task,
          id: this.id,
          completed: this.completed
        })
      }
      this.id++
      this.task = ''
      console.log(this.id)
    },
    deleteTodo(id){
      this.tasks = this.tasks.filter(el => el.id !== id)
    },
    deleteCompleted(){
      // this.tasks = this.tasks.filter(el => el.id !== id)
      this.tasks = this.tasks.filter(el => !el.completed)

    },
    isCompleted(){
      this.completed = true;
      console.log('isCompleted')
    },
    isActive(){
      this.completed = false;
      console.log('isActive')
    },
    isAll(){
      this.tasks;
      console.log('isAll')
    },
    checkAll(){
      if(!this.completed){
        for(var key in this.tasks){
          this.tasks[key].completed = true
        }
      }
    },
    UnCheckAll(){
      if(this.completed){
        for(var key in this.tasks){
          this.tasks[key].completed = false
        }
      }

    }
    // filtered () {
    //   var filtered = this.tasks
    //   this.tasks.forEach(filter => {
    //     filtered = filtered.filter(record => {
    //       return filter.completed === true
    //           ? new RegExp(filter.value, 'i').test(record[filter.completed])
    //           : record[filter.completed] == filter.value
    //     })
    //   })
    //   return filtered
  },
  components: {

  },
  computed: {
    activeTasks() {
      let activeTasks = this.tasks.filter((el) => {
        return el.completed == false;
      })
      return activeTasks;
    },
    completedTasks() {
      let completedTasks = this.tasks.filter((el) => {
        return el.completed == true;
      })
      return completedTasks;
    },
    getTasks(){
      if(this.completed) {
        return this.tasks.filter((el) => el.completed === true)
      }
      else if(!this.completed) {
        return this.tasks.filter((el) => el.completed === false)
      }
        return this.tasks
    }

      // this.tasks.map(el => el.completed === true)
      // // this.tasks.forEach(filter => {
      // //   // filtered = filtered.filter(record => {
      // //   return filter.completed === true
      // //     // ? new RegExp(filter.value, 'i').test(record[filter.completed])
      // //     //     : record[filter.completed] == filter.value
      // //   // })
      // // })
      // console.log('hello'+this.tasks.map(el => el.completed === true))
      // return filtered
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
