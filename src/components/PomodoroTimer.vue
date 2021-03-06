<template>
  <div class="pomodoro-timer">
   
    <div class="pomodoro-timer__screen">
      {{ formatedTime(remainingTimeInMilliseconds) }}
    </div>
    <div class="pomodoro-timer__buttons">
      <v-btn
        v-if="state === 'IDLE'"
        @click="start">
        start
      </v-btn>
      <v-btn
        v-if="state === 'TICKING'"
        @click="pause">
        pause
      </v-btn>
      <v-btn
        v-if="state === 'PAUSED'"
        @click="start">
        resume
      </v-btn>
      <v-btn @click="reset">
        reset
      </v-btn>
    </div>
     <div class="pomodoro-timer__buttons">
      <template v-for="name in periodNames">
        <v-btn
          :key="name"
          :selected="selectedPeriodName === name"
          @click="selectPeriod(name)">
          {{ name }}
        </v-btn>
      </template>
    </div>
  </div>
</template>

<script>
import TimerButton from '@/components/TimerButton.vue'
import parseMilliseconds from 'parse-ms'

const MINUTE_TO_MILLISECOND = 60000

function padLeft (value, number, char) {
  let text = String(value)
  if (text.length < number) {
    let size = number - text.length
    while (size--) text = char + text
  }
  return text
}

export default {
  name: 'Timer',
  components: {
    TimerButton
  },
  props: {
    timeByPeriodInMinute: {
      type: Object,
      required: false,
      default () {
        return { letswork: 25, shortBreak: 5, longBreak: 20 }
      }
    },
    initialNumberWorkPeriodCompleted: {
      type: Number,
      required: true
    },
    autoStartEnabled: {
      type: Boolean,
      required: false,
      default: false
    },
    tickIntervalInMilliseconds: {
      type: Number,
      required: false,
      default: 250
    }
  },
  data () {
    const selectedPeriodName = 'work'
    return {
      selectedPeriodName,
      remainingTimeInMilliseconds: this.timeByPeriodInMinute[selectedPeriodName] * MINUTE_TO_MILLISECOND,
      numberWorkPeriodCompleted: this.initialNumberWorkPeriodCompleted,
      periodNames: ['letswork', 'shortBreak', 'longBreak'],
      state: 'IDLE',
      intervalId: null,
      lastTickAt: null
    }
  },
  computed: {
    selectedPeriodDurationInMilliseconds () {
      return this.timeByPeriodInMinute[this.selectedPeriodName] * MINUTE_TO_MILLISECOND
    }
  },
  methods: {
    start () {
      this.state = 'TICKING'
      this.lastTickAt = Date.now()
      this.tick()
      this.intervalId = setInterval(this.tick, this.tickIntervalInMilliseconds)
    },
    pause () {
      this.state = 'PAUSED'
      clearInterval(this.intervalId)
      this.intervalId = null
    },
    reset () {
      this.state = 'IDLE'
      clearInterval(this.intervalId)
      this.intervalId = null
      this.remainingTimeInMilliseconds = this.selectedPeriodDurationInMilliseconds
      this.$emit('change', this.formatedTime(this.remainingTimeInMilliseconds))
    },
    completed () {
      this.$emit('completed', this.selectedPeriodName, this.selectedPeriodDurationInMilliseconds)
      this.state = 'IDLE'
      clearInterval(this.intervalId)
      this.intervalId = null
      // Change period.
      switch (this.selectedPeriodName) {
        case 'short':
        case 'long':
          this.selectedPeriodName = 'work'
          break
        case 'work':
          this.numberWorkPeriodCompleted += 1
          if (this.numberWorkPeriodCompleted % 4 === 0) {
            this.selectedPeriodName = 'long'
          } else {
            this.selectedPeriodName = 'short'
          }
      }
      this.remainingTimeInMilliseconds = this.selectedPeriodDurationInMilliseconds
      this.$emit('change', this.formatedTime(this.remainingTimeInMilliseconds))
      // Start period if enabled.
      if (this.autoStartEnabled) {
        setTimeout(() => this.start(), 1000)
      }
    },
    tick () {
      let now = Date.now()
      let elapsedMilliseconds = now - this.lastTickAt
      this.remainingTimeInMilliseconds = this.remainingTimeInMilliseconds - elapsedMilliseconds
      this.lastTickAt = now

      if (this.remainingTimeInMilliseconds > 0) {
        this.$emit('change', this.formatedTime(this.remainingTimeInMilliseconds))
      } else {
        this.completed()
      }
    },
    selectPeriod (periodName) {
      this.reset()
      this.selectedPeriodName = periodName
      this.remainingTimeInMilliseconds = this.selectedPeriodDurationInMilliseconds
      this.$emit('change', this.formatedTime(this.remainingTimeInMilliseconds))
    },
    formatedTime (timeInMilliseconds) {
      const duration = parseMilliseconds(timeInMilliseconds)
      const { minutes, seconds } = duration
      return [ minutes, seconds ].map((value) => padLeft(value, 2, '0')).join(':')
    }
  },
  mounted () {
    this.$emit('change', this.formatedTime(this.remainingTimeInMilliseconds))
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

$timeBoxShadowHeigh: 20px;
$color-text: #4a4a4a;

.pomodoro-timer {
  display: flex;
  flex-direction: column;
  align-items: center;

  &__buttons {
    display: flex;
    align-items: center;
    justify-content: center;
  }

  &__screen {
    margin-bottom: $timeBoxShadowHeigh;
    display: inline-block;
    padding: 100px;
    font-size: 3.5em;
    
    border: 3px solid $color-text;
    border-radius: 400px;
    box-shadow: 0 $timeBoxShadowHeigh $color-text;
 
  }
}
</style>
