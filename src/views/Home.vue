<template>


 <v-container>
  <v-app>
    <v-row>
    <v-col>
    
    <v-content>
      <Navbar />
 </v-content>
       
      </v-col>
    </v-row>
      </v-app>
  </v-container>

   
   

  
     

</template>

<script>
  
  import Navbar from '@/components/Navbar'



  export default {
    name: 'Home',

    components: {
     
      Navbar
    },
  }
</script>
