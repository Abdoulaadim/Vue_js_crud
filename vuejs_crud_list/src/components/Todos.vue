<template>
<div>
       <h1>List of Todos</h1>

    
         <div class="form-group mb-2">
          <input type="text" v-model="title" placeholder="New todo ..." class="form-control">
          </div>
           <button @click="addTodo" class="btn btn-block btn-success mb-2 ">Add Todo</button> 
          <button v-if="myTodo" @click="updateTodo" class="btn btn-block btn-warning mb-2">Update Todo</button> 

         
    
  


    <ul class="list-group">
        <li class="list-group-item" v-bind:key="todo.id" v-for="todo in todos">
            {{todo.title}}

            <Todo :todo="todo" v-on:deleteTodo="deleteOnTodo" @updateTodo="editTodo"/>

        </li>

    </ul> 
</div>

</template>

<script>
import axios from 'axios';
import Todo from './Todo'
export default {

  name:"Todos",
  components:{
    Todo
  
  },
  data(){
    return {
      title:'',
      myTodo:'',
      todos:[]
    }
  },
  methods:{
    updateTodo(){
         let todo={
                      ...this.myTodo,
                      title:this.title
        }
        axios.put('http://localhost:5000/todos/'+todo.id, todo)
        .then(res=> {this.todos=this.todos.map(todo => {

            if(res.data.id === todo.id) {

            return res.data;
          }
            return todo;
        })
        this.title='';
        this.myTodo=null;
        
       });
    
     
    },
    addTodo(){
      //  e.preventDefault();
     
      if(this.title != '' ){
        let newTodo={
        title:this.title,
        completed:false

      }
      axios.post('http://localhost:5000/todos', newTodo)
      .then(res=> {this.todos= 
      [res.data , ...this.todos];
      this.title='';
      }) 

      }

    },
    editTodo(todo){
        this.myTodo=todo;
        this.title=todo.title;
    },
    getTodos(){
      axios.get('http://localhost:5000/todos')
      .then(res=>this.todos=res.data)
      .catch(err=>console.log(err))
    },
    deleteOnTodo(id){

      if(confirm ('Are you to delete this todo ?')){
          axios.delete(`http://localhost:5000/todos/${id}`)
         .then(()=>{
         this.todos=this.todos.filter(todo=>todo.id !==id);
        })
      }
      
    }

  },
  created(){
    
    this.getTodos();
  
  },
 
}
</script>


<style>

</style>