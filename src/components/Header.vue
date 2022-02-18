<template>
<div class ="header-container">
    <h1>Task Tracker</h1>
    <button 
    class ="btn"
    v-show="homePage"
    @click="$emit('toggle-add-task')"
    :style="{background:color}">{{text}}</button>
</div>
</template>

<script>
export default {
    name:'Header',
    props :{
        text :String,
        color:String
    },
    computed:{
        homePage(){
            if(this.$route.path==='/'){
                return true
            }
            else{
                return false
            }
        }
    }
}
</script>

<style scoped>
.header-container{
    display: flex;
    align-items: center;
    justify-content: space-between;
}
</style>