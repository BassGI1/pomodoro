<script>
  export default {
    name: 'App',
    data() {
      return {
        timer: 5,
        interval: false,
        timeout: false,
        stopped: true
      }
    },
    methods: {
      count: function() {
        this.interval = setInterval(() => {
          --this.timer
        }, 1000)
        this.stopped = !this.stopped
        if (this.interval){
          this.timeout = setTimeout(() => clearInterval(this.interval), this.timer*1000)
        }
      },
      clear: function() {
        clearTimeout(this.timeout)
        clearInterval(this.interval)
        this.stopped = !this.stopped
      }
    },
    mounted() {
      this.count()
    }
  }
</script>



<template>
  <div>
    {{timer}}
  </div>
  <button v-if="!stopped" @click="clear">
    Stop
  </button>
  <button v-if="stopped" @click="count">
    Start
  </button>
</template>



<style>

</style>