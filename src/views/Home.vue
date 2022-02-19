<template>
    <AddTask v-show="showAddTask"
   @add-task="addTask"/>
   <AllTasks 
   @toggle-reminder ="toggleReminder"
   @delete-task ="deleteTask"
   :tasks="tasks"/>
</template>

<script>
import AllTasks from '../components/AllTasks'
import AddTask from '../components/AddTask'


export default {
    name : 'Home',
    components:{
        AllTasks,
        AddTask
    },

    data(){
        return{
            tasks:[],
        }
    },

    props:{
        showAddTask :Boolean
    },
      methods:{
    async addTask(task){
      const url = 'http://localhost:5000/tasks'
      const options ={
        method:'POST',
        headers : {
          'Content-type' : 'application/json',
        },
        body : JSON.stringify(task)
      }
      const response = await fetch(url,options)
      const data = await response.json()
      this.tasks = [...this.tasks,data]
    },
    async deleteTask(id){
      if (confirm('Are you sure ?')){
        const url = `http://localhost:5000/tasks/${id}`
        const options={
          method:'DELETE',
        }
        const response = await fetch(url,options)
        response.status === 200 ?
         (this.tasks = this.tasks.filter((each)=>each.id!==id))
         : alert('Error Deleting Task')
      }
    },
    async toggleReminder(id){
      const taskToToggle = await this.fetchTask(id)
      const updatedTask = {...taskToToggle,reminder:!taskToToggle.reminder}
       const url = `http://localhost:5000/tasks/${id}`
      const options ={
        method:'PUT',
        headers : {
          'Content-type' : 'application/json',
        },
        body : JSON.stringify(updatedTask)
      }
      const res = await fetch(url,options)
      const data = await res.json()
        this.tasks = this.tasks.map((task)=>task.id === id ? 
          {...task,reminder : data.reminder} : task
        )
    },
    async fetchTasks() {
      const response = await fetch('http://localhost:5000/tasks')
      const data = await response.json()
      return data
    },
    async fetchTask(id) {
      const response = await fetch(`http://localhost:5000/tasks/${id}`)
      const data = await response.json()
      return data
    }
  },
  async created(){
    this.tasks= await this.fetchTasks()
  },

    // methods:{
    //     addTask(task){
    //       this.tasks =[...this.tasks, task]  
    //       // localStorage.setItem('data',JSON.stringify(this.tasks))
    // }, 
    // deleteTask(id){
    //     if(confirm('Are you sure ?')){
    //    this.tasks=this.tasks.filter((each)=>each.id!==id)
    //  } },

    // toggleReminder(id){
    //    this.tasks = this.tasks.map((task)=>task.id === id ? 
    //       {...task,reminder : !task.reminder} : task
    //     )},
    // },

    // created(){
    // this.tasks =[
    //   {
    //   "id": "1",
    //   "text": "Doctors Appointment",
    //   "day": "March 5th at 2:30pm",
    //   "reminder": true
    // },
    // {
    //   "id": "2",
    //   "text": "Meeting with boss",
    //   "day": "March 6th at 1:30pm",
    //   "reminder": false
    // },
    // {
    //   "id": "3",
    //   "text": "Food shopping",
    //   "day": "March 7th at 2:00pm",
    //   "reminder": true
    // },
    // ]
    // },
    // mounted(){
    //   this.tasks = JSON.parse(localStorage.getItem('data'))
    // }
  
}
</script>