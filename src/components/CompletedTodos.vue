<template>
    <div id="completed-todos" class="container">
        <h3 v-if="completed.length > 0">{{completed.length}} Completed Task </h3>
        <ul class="list-group">
        <li class="list-group-item" v-for="todo in completed">
                {{todo.body}}
                <button type="button" @click="remove(todo)" class="btn btn-default btn-sm">
                
                </button>
        </li>
        </ul>

    </div>
</template>

<script>
    export default{


        methods: {
            remove(todo){
                this.$store.dispatch('removeTodo', todo)
            }
        },
 
        computed: {
            completed(){
                return this.$store.getters.completedTodos
            }
        }
    }
</script>

<style>

#completed-todos {
padding-left: 30px;
border: none;
padding: 15px 32px;

text-decoration: none;
display: inline-block;
font-size: 16px;
}

h3 {
font-size: 29px;
}



</style>