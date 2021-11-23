<template>
<div>
            <h2 :class="{'is-completed':todo.completed}">{{todo.title}}</h2>
            <div class="text-end">
                <button @click="updateMyTodod" class="btn btn-info btn-sm mx-1">Edit</button>
                <button @click="deleteTodo" class="btn btn-danger btn-sm">Delete</button>
            </div>
</div>
         
</template>

<script>
export default {
    props:['todo'],
    methods:{
        deleteTodo(){
            this.$emit('deleteTodo',this.todo.id);
        },
        updateMyTodod(){
            this.$emit('updateTodo',this.todo)
        },
    },

}
</script>

<style>
.is-completed{
    text-decoration:line-through ;
}
</style>