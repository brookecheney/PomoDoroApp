<template>
    <div id="get-todo" class="container">
 
        <span class="white--text font-weight-bold text-uppercase">Lap</span>
      
        <v-col cols="12" md="6">
       
      </v-col>
      <v-container fluid>
       <v-row>
      <v-col cols="12" md="6">
   
        <input class="form-control"
               :value="newTodo"
               @change="getTodo"
               placeholder="Enter Task">
       
       <v-btn class="error" @click="addTodo">
       <span class="white--text font-weight-bold text-uppercase">Add Task</span></v-btn>
     </v-col>
    </v-row>
     </v-container>
    </div>
</template>
<script>
    export default{
        methods: {
            getTodo(e){
                this.$store.dispatch('getTodo', e.target.value)
            },
            addTodo(){
                this.$store.dispatch('addTodo')
                this.$store.dispatch('clearTodo')
            }
        },
        computed: {
            newTodo(){
                return this.$store.getters.newTodo
            }
        }

    }
</script>

<style>
 v-btn nice {

color:#5E35B1;
padding: 80px;
padding-bottom: 35px;

 }
.form-control {

  border: 1px solid #888; 
  padding: 15px;
  padding-right: 200px;
}
</style>