<template>
  <div id="app">

    <div class="timmer">

      <div class="timmer-header">
        <span>Pomodoro</span>
        <button class="btn" @click="start" :disabled="state === 'started'">Start</button>
        <button class="btn btn-pause" @click="pause" :disabled="state !== 'started'">Pause</button>
        <button class="btn btn-stop" @click="stop" :disabled="state !== 'started' && state !=='paused'">Stop</button>
        <br>
        {{title}}
      </div>

      <hr>

      <div class="timmer-log">
        {{min}}:{{sec}}
      </div>

    </div>
  </div>
</template>

<script>
const TIME_STATES = {
  WORK: 'work',
  REST: 'rest'
};

const STATES = {
  STARTED: 'started',
  STOPPED: 'stopped',
  PAUSED: 'paused'
}

const WORKING_TIME_IN_MINUTES = 1;
const RESTING_TIME_IN_MINUTES = 1;

export default {
  name: 'app',
  data() {
    return {
      state: STATES.STOPPED,
      minute: WORKING_TIME_IN_MINUTES,
      seconds: 0,
      timeState: TIME_STATES.WORK,
      timestamp: 0      
    }
  },
  computed: {
    title() {
      return this.timeState === TIME_STATES.WORK ? 'Work!' : 'Rest!';
    },
    min(){
      if(this.minute < 10) {
        return `0${this.minute}`;
      }
      return this.minute;
    },
    sec() {
      if(this.seconds < 10){
         return `0${this.seconds}`;
      }
       return this.seconds;
    }
  },
  methods: {
    start() {
      this.state = STATES.STARTED;
      this._tick();
      this.interval = setInterval(this._tick, 1000);
    },
    pause(){      
      this.state = STATES.PAUSED;
      clearInterval(this.interval);
    },
    stop(){
      this.state = STATES.STOPPED;
      clearInterval(this.interval);
      this.timeState = TIME_STATES.WORK;
      this.minute = WORKING_TIME_IN_MINUTES;
      this.seconds = 0;
    },
    _tick() {
      //if still seconds decerement
      if(this.seconds !== 0) {
        this.seconds--;
        return;
      }

      //if seconds = 0 lower miniutes and set seconds to 59
      if(this.minute !== 0) {
        this.minute--;
        this.seconds = 59;
        return;
      }

      //seconds & minutes are 0
      //toggle working/rest state
      this.timeState = this.timeState === TIME_STATES.WORK ? TIME_STATES.REST : TIME_STATES.WORK;
      
      if(this.timeState === TIME_STATES.WORK) {
        this.minute = WORKING_TIME_IN_MINUTES;
      }else {
        this.minute = RESTING_TIME_IN_MINUTES;
      }
      
    }
  }
}
</script>

<style lang="scss">

.timmer {
  max-width: 350px;
  margin: 0 auto;
  padding: 15px;
  border: 1px solid black;
  text-align: left;
  
  &-log {
    padding: 5px;
    color: white;
    background: grey;
  }

  .btn {
    padding: 10px;
    background: lightblue;
    border: 1px solid lightcyan;
    color: white;
    margin: 0 10px;
    cursor: pointer;

    &:disabled {
      opacity: .5;
      cursor: not-allowed;
    }

    &-pause {
      background: grey;
    }

    &-stop {
      background: red;
    }
  }
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
