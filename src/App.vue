<template>

 <v-app>

   <Navbar />
   <br>
   <br>
   <br>
   <v-content>
        <div class="row">
  <div class="column">
    

  </div>
  <div class="column">
     
     <PomodoroTimer />
     <br>
     <h2>TOTAL POMOS : {{ numberWorkPeriodToday }}</h2>
<GetTodo />
        <CurrentTodos />
      <CompletedTodos />
  </div>
  <div class="column">
   
  </div>
</div>
    
 
    </v-content>
 </v-app>
</template>

<script>
import { mapActions } from 'vuex'
import Timer from '@/components/Timer'
import Navbar from '@/components/Navbar'
import CompletedTodos from '@/components/CompletedTodos'
import GetTodo from '@/components/GetTodo'
import PomodoroTimer from '@/components/PomodoroTimer'
import CurrentTodos from '@/components/CurrentTodos'

export default {
  components: {
    GetTodo,
     PomodoroTimer,
      Navbar,
      Timer,
      CurrentTodos,
      CompletedTodos
    },
    data () {
    return {
      timerState: 'stopped',
      currentTimer: 0,
      formattedTime: "00:00:00",
      ticker: undefined,
      laps: [],
      latestLap: "",
      snackbar: false
    }
  },
    computed: {
      cols () {
        const { lg, sm } = this.$vuetify.breakpoint
        return lg ? [3, 9] : sm ? [9, 3] : [6, 6]
      },
    },
 
    methods: {
        ...mapActions([
      'settingsFetch',
      'periodsFetch'
    ]),
    start () {
      if (this.timerState !== 'running') {
        this.tick();
        this.timerState = 'running';
      }
    },
    lap () {
      this.snackbar = true;
      this.laps.push({
        seconds: this.currentTimer,
        formattedTime: this.formatTime(this.currentTimer)
      });
      this.latestLap = this.formatTime(this.currentTimer);
      this.currentTimer = 0;
    },
    pause () {
      window.clearInterval(this.ticker);
      this.timerState = 'paused';
    },
    stop () {
      window.clearInterval(this.ticker)
      this.currentTimer = 0;
      this.formattedTime = "00:00:00";
      this.timerState = "stopped";
    },
    tick () {
      this.ticker = setInterval(() => {
        this.currentTimer++;
        this.formattedTime = this.formatTime(this.currentTimer);
      }, 250)
    },
    formatTime (seconds) {
      let measuredTime = new Date(null);
      measuredTime.setSeconds(seconds);
      let MHSTime = measuredTime.toISOString().substr(11, 8);
      return MHSTime;
    }
  },
  mounted () {
    this.settingsFetch()
    this.periodsFetch()
  }
}
</script>

<style lang="scss">
$color-background: rgb(110, 240, 84);
* {
  box-sizing: border-box;
}

/* Create three equal columns that floats next to each other */
.column {
  float: left;
  width: 33.33%;
  padding: 10px;
  height: 300px; /* Should be removed. Only for demonstration */
}
h2 {
 padding-left: 20px;
}

/* Clear floats after the columns */
.row:after {
  content: "";
  display: table;
  clear: both;
}

</style>
