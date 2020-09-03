<template>
  <section class="app">
    <header>
      <h1>todos</h1>
      <div class="one-line">

        <button >
          <img v-if="tasks.length === 0" @click="checkAll" src="../src/assets/tick.svg">
          <img v-else-if="tasks.length === completedTasks.length" @click="UnCheckAll" src="../src/assets/delete-all.svg">
          <img v-else @click="checkAll" src="../src/assets/tick.svg">

        </button>
        <input class="add-todo" v-model="task" @keypress.enter="addTodo" type="text"
               placeholder="What needs to be done?"
               autofocus>
      </div>

    </header>
    <section class="main">
      <!--      <input class="check-all" @click="checkAll" type="checkbox">-->
      <!--      <label for="check-all"></label>-->
      <ul class="todo-list">
        <li class="completed" v-for="task in getTasks" v-bind:key="task.id">
          <div class="view">
            <label>
              <input class="toggle" v-model="task.completed" type="checkbox">
              <span class="checkmark"></span>
            </label>
              <del class="todo-completed" v-if="task.completed == true">{{ task.task }}</del>
            <label v-else="">{{ task.task }}</label>
            <button @click="deleteTodo(task.id)">
              <img src="../src/assets/cross.svg">
            </button>
          </div>
        </li>
      </ul>
    </section>
    <footer class="footer">
      <span class="todo-count">
        <strong>{{ tasks.length }}</strong> items left
      </span>
      <ul class="filters">
        <li>
          <a @click="filter = 'all'">All</a>
          <!--          <button value="all" @click="filter = 'all'">All</button>-->
        </li>
        <li>
          <a @click="filter = 'active'">Active</a>
          <!--          <button value="all" @click="filter = 'all'">All</button>-->
        </li>
        <li>
          <a @click="filter = 'completed'">Completed</a>
          <!--          <button value="all" @click="filter = 'all'">All</button>-->
        </li>
      </ul>
      <button @click="deleteCompleted(task.id)" v-if="completedTasks.length > 0" class="clear-completed">Clear Completed</button>
    </footer>

  </section>

</template>


<script>

export default {
  name: 'App',
  data() {
    return {
      id: 0,
      task: '',
      filter: 'all',
      tasks: []
    }
  },
  methods: {
    addTodo() {
      if (this.task.length > 0) {
        this.tasks.push({
          task: this.task,
          id: this.id,
          completed: false
        })
      }
      this.id++
      this.task = ''
      console.log(this.id)
    },
    deleteTodo(id) {
      this.tasks = this.tasks.filter(el => el.id !== id)
    },
    deleteCompleted() {
      // this.tasks = this.tasks.filter(el => el.id !== id)
      this.tasks = this.tasks.filter(el => !el.completed)

    },
    checkAll() {
      for (var key in this.tasks) {

        this.tasks[key].completed = true;
      }
    },
    UnCheckAll() {
      let allCompletedTasks = this.tasks.filter(task => task.completed)
      // let allCompletedTasks = this.tasks.filter(el => el.filter === 'completed')
      console.log('1212'+ allCompletedTasks)
      for (var key in this.tasks) {
        if (this.tasks.length === allCompletedTasks.length) {
          this.tasks[key].completed = false
        }
      }
    },
  },
  components: {},
  computed: {
    activeTasks() {
      let activeTasks = this.tasks.filter((el) => {
        return el.completed == false;
      })
      return activeTasks;
    },
    completedTasks() {
      let allCompletedTasks = this.tasks.filter(task => task.completed)
      return allCompletedTasks;
    },
    getTasks() {
      if (this.filter === 'completed') {
        return this.tasks.filter((el) => el.completed === true)
      } else if (this.filter === 'active') {
        return this.tasks.filter((el) => el.completed === false)
      }
      return this.tasks
    }
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
