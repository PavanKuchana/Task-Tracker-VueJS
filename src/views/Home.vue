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
            tasks:[]
        }
    },

    props:{
        showAddTask :Boolean
    },

    methods:{
        addTask(task){
         this.tasks =[...this.tasks, task]
    },
     
    deleteTask(id){
        if(confirm('Are you sure ?')){
      this.tasks=this.tasks.filter((each)=>each.id!==id)
     } },

    toggleReminder(id){
       this.tasks = this.tasks.map((task)=>task.id === id ? 
          {...task,reminder : !task.reminder} : task
        )},
    },

    created(){
    this.tasks =[
      {
      "id": "1",
      "text": "Doctors Appointment",
      "day": "March 5th at 2:30pm",
      "reminder": true
    },
    {
      "id": "2",
      "text": "Meeting with boss",
      "day": "March 6th at 1:30pm",
      "reminder": false
    },
    {
      "id": "3",
      "text": "Food shopping",
      "day": "March 7th at 2:00pm",
      "reminder": true
    },
    ]
    },
  
}
</script>