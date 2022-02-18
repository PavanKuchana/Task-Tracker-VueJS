<template>
    <div :key ="task.id" v-for="task in tasks">
        <EachTask 
        :task="task" 
        @delete-task="$emit('delete-task',task.id)"
        @toggle-reminder ="$emit('toggle-reminder',task.id)"
        />
    </div>
</template>


<script>

import EachTask from './EachTask'

export default {
    name:'AllTasks',
    components:{
        EachTask
    },
    props:{
        tasks : Array
    },
    emits :['delete-task','toggle-reminder']
}
</script>

<style scoped>

</style>