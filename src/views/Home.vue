<template>
    <div v-show="showAddTask" >
      <AddTask @add-task="addTask" />
    </div>
    <TasksItem @toggle-reminder="toggleReminder" @delete-task="deleteTask" :tasks="tasks"/>
</template>

<script>
import TasksItem from '../components/Tasks'
import AddTask from '../components/AddTask'

export default {
    name: 'HomeView',
    props: {
        showAddTask: Boolean 
    },
    components: {
        TasksItem,
        AddTask
    },
    data(){
        return{
            tasks: [],
        }
    },
    methods: {
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