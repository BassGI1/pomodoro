<script>
  import Rest from './components/Rest.vue'
  const alarm = new Audio("/assets/alarm.wav")
  export default {
    name: "App",
    data() {
        return {
            timer: 1500,
            interval: false,
            timeout: false,
            stopped: true,
            changeColor1: false,
            changeColor2: false,
            renderRest: false,
            returnTimer: false,
            settingsForward: false,
            settingsBackward: false,
            renderSettings: false,
            background: 'rgb(108, 205, 108)'
        };
    },
    methods: {
        count: function() {
            this.changeColor1 = true;
            this.changeColor2 = false;
            this.interval = setInterval(() => {
                --this.timer;
            }, 1000);
            this.stopped = !this.stopped;
            if (this.interval) {
                this.timeout = setTimeout(() => {
                    alarm.play();
                    clearInterval(this.interval);
                    this.changeColor2 = true;
                    this.renderRest = true
                }, this.timer * 1000);
            }
        },
        clear: function() {
            this.changeColor1 = false;
            this.changeColor2 = true;
            clearTimeout(this.timeout);
            clearInterval(this.interval);
            this.stopped = !this.stopped;
        },
        restart: function() {
          this.renderRest = false
          this.timer = 1500
          this.count()
          this.stopped = false
          this.returnTimer = true
          setTimeout(() => {this.returnTimer = false}, 2000)
        },
        restCount: function() {
          this.timer = 5*60
          this.count()
        },
        moveSetting: function() {
          if (!this.settingsForward){
            this.settingsBackward = false
            this.settingsForward = true
            setTimeout(() => {this.renderSettings = true}, 2000)
          }
          else{
            this.settingsForward = false
            this.settingsBackward = true
            this.renderSettings = false
          }
        }
    },
    computed: {
      cssVars() {
        return {'--bg': this.background}
      }
    },
    components: { Rest }
};
</script>

<template>
  <div
    class="parent"
    v-bind:class="{ changeColor1: changeColor1, changeColor2: changeColor2 }"
    :style="cssVars"
  >
    <h1 class="timer" v-bind:class="{'restTimer': renderRest, 'returnTimer': returnTimer}">{{ Math.trunc(timer/60) }}:{{ JSON.stringify(timer%60).length == 2 ? timer%60 : `0${JSON.stringify(timer%60)}` }}</h1>
  </div>
  <img src="/assets/play.png" alt="play" v-if="!stopped && timer !== 0 && !renderRest" @click="clear" class="button">
  <img src="/assets/stop.png" alt="stop" v-if="stopped && timer !== 0 && !renderRest" @click="count" class="button">
  <img src="/assets/settings.png" alt="settings" v-if="timer !== 0 && !renderRest" class="button" style="width: 7.5vh; left: 6vw;" @click="moveSetting" v-bind:class="{'settingsForward': settingsForward, 'settingsBackward': settingsBackward}">
  <div class="settings" v-if="renderSettings">
    <img src="/assets/green.png" alt="green" class="colour" @click="background = 'rgb(108, 205, 108)'">
    <img src="/assets/blue.png" alt="blue" class="colour" @click="background = 'rgb(66, 169, 232)'">
    <img src="/assets/black.png" alt="black" class="colour" @click="background = 'rgb(44, 40, 40)'">
    <img src="/assets/yellow.png" alt="yellow" class="colour" @click="background = 'rgb(233, 248, 121)'">
    <img src="/assets/pink.png" alt="pink" class="colour" @click="background = 'rgb(247, 118, 217)'">
  </div>
  <Rest v-if="renderRest === true" @restart="restart" @rest="restCount"/>
</template>

<style>
  .parent {
    cursor: default;
    width: 100vw;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: rgb(247, 118, 118); 
    color: white;
  }
  .button {
    position: absolute;
    left: 1vw;
    bottom: 1vw;
    width: 3vw;
    height: 7.5vh;
    cursor: pointer;
  }
  .changeColor1 {
    animation: changeColor1 0.75s forwards;
  }
  @keyframes changeColor1 {
    100% {
      background-color: var(--bg);
    }
  }
  .changeColor2 {
    animation: changeColor2 0.75s forwards;
  }
  @keyframes changeColor2 {
    0% {
      background-color: var(--bg);
    }
    100% {
      background-color: rgb(247, 118, 118);
    }
  }
  .timer{
    font-family: 'Roboto', sans-serif;
    font-size: 11rem;
  }
  .restTimer{
    animation: restTimer 2s forwards;
  }
  @keyframes restTimer{
    0%{
      font-size: 11rem;
    }
    100%{
      transform: translateY(-35vh);
      font-size: 7rem;
    }
  }
  .returnTimer{
    animation: returnTimer 2s forwards;
  }
  @keyframes returnTimer{
    0%{
      transform: translateY(-35vh);
      font-size: 7rem;
    }
    100%{
      font-size: 11rem;
    }
  }
  .settingsForward{
    animation: settingsForward 2s forwards;
  }
  @keyframes settingsForward{
    100%{
      transform: translateX(60vw) rotate(360deg);
    }
  }
  .settingsBackward{
    animation: settingsBackward 2s both;
  }
  @keyframes settingsBackward {
    0%{
      transform: translateX(60vw);
    }
    100%{
      transform: translateX(0) rotate(360deg);
    }
  }
  .settings{
    position: absolute;
    left: 5vw;
    bottom: 1vw;
    height: 7.5vh;
    width: 60vw;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .colour{
    width: 20%;
    height: 100%;
    cursor: pointer;
  }
</style>
