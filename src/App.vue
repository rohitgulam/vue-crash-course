<template>
  <div class="container">
    <HeaderItem 
    @toggle-add-task="toggleAddTask" 
    title="Task Tracker" 
    :showAddTask="showAddTask" />
    <div v-if="showAddTask" >
      <AddTask @add-task="addTask" />
    </div>
    <TasksItem @toggle-reminder="toggleReminder" @delete-task="deleteTask" :tasks="tasks"/>
  </div>
</template>

<script>
import HeaderItem from './components/Header'
import TasksItem from './components/Tasks'
import AddTask from './components/AddTask'


export default {
  name: 'App',
  components: {
    HeaderItem,
    TasksItem,
    AddTask
  },
  data(){
    return{
      tasks: [],
      showAddTask: false,
    }
  },
  methods: {
    toggleAddTask(){
      this.showAddTask = !this.showAddTask
    },
    async addTask(task){
      const res = await fetch('api/task', {
        method: 'POST',
        header:{
          'Content-Type': 'apllication/json',
        }, 
        body: JSON.stringify(task)
      })

      const data = await res.json()
      this.tasks = [...this.tasks, data]
    },
    deleteTask(id){
      this.tasks = this.tasks.filter((task) => {
        task.id != id
      })
    },
    toggleReminder(id){
      this.tasks = this.tasks.map((task) => task.id === id ? {...task, reminder: !task.reminder} : task)
    },
   async fetchTasks(){
    const res = await fetch('api/tasks')
    const data = await res.json()
    return data
   }
  },

   async fetchTask(id){
    const res = await fetch(`api/tasks/${id}`)
    const data = await res.json()
    return data
   },
  async created() {
    this.tasks = await this.fetchTasks()
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap');
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: 'Poppins', sans-serif;
}
.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}
.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}
.btn:focus {
  outline: none;
}
.btn:active {
  transform: scale(0.98);
}
.btn-block {
  display: block;
  width: 100%;
}
</style>
