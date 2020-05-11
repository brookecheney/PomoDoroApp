<template>
	<v-container fluid>
    <v-layout>
      <v-flex xs12 pa-3>
        <v-card>
          <v-card-title class="primary white--text font-weight-light display-2 text-uppercase justify-center">
            {{ timer }}
          </v-card-title>
         
            <v-list>

                        <div>
    <div class="d-block pa-2 success mb-3 white--text" @click="$emit('start')"
                :disabled="state == 'running'">Start</div>
    
  </div>

              
            
     <div class="d-block pa-2 info mb-3 white--text"      
                @click="$emit('lap')"
                :disabled="state == 'stopped' || state == 'paused'">
              Pomo
               </div>
             
              <div
                class="d-block pa-2 warning mb-3 white--text"
                @click="$emit('pause')"
                :disabled="state == 'stopped' || state == 'paused'"
              >Pause
              </div>
              
              <div
                class="d-block pa-2 error white--text"
                @click="$emit('stop')"
                :disabled="state == 'stopped'"
              >
                Stop
            </div>
           
            </v-list>
        
          
        </v-card>
    
                 
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>


export default {
  name: 'Timer',
  props: ['timer', 'state']
}
</script>
