<template>
    <div id="current-todos" class="container">
        <span class="font-weight-white secondary--text">
        <h3 v-if="todos.length > 0">{{todos.length}} Task in Process</h3></span>
     
        <div>

         
        <ul class="list-group">
        
            <li class="list-group-item" v-for="todo in todos">
                {{todo.body}}
            <div class="btn-group">
                
           
        
        
           <v-btn>
           
                <button type="button" @click="complete(todo)" class="btn btn-default btn-sm">
                <span class="glyphicon glyphicon-ok-circle"></span> Complete
                </button> 
               </v-btn>
               <v-btn>       
                <button type="button" @click="remove(todo)" class="btn btn-default btn-sm">
                <span class="glyphicon glyphicon-remove-circle"></span>remove
                </button>
             </v-btn>
            </div>
            </li>
        </ul>
        
        </div>
        <br>
     
        <br>
    </div>

</template>
<script>

export default {
  

    methods: {
        edit(todo){
            this.$store.dispatch('editTodo', todo)
        },
        complete(todo){
            this.$store.dispatch('completeTodo', todo)
        },
        remove(todo){
            this.$store.dispatch('removeTodo', todo)
        }
    },
    computed: {
        todos(){
            return this.$store.getters.todos
        }
    }
}
</script>
<style>
h3 {
    font-weight: 300px;
}
.list-group-item {
  padding: 15px 32px;
  margin:20px;
  font-size: 20px;

}

    .btn-group{
        float: right;
    }
    .btn-default btn-sm {
 background-color: #4CAF50; /* Green */
  border: none;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
}
.button2 {
  
 background-color: #4CAF50; /* Green */
  border: none;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
}
    h3{
    font-weight: 900;
    }
</style>