<template>
  <div>
    <img alt="Vue logo" src="../assets/logo.png">
    <div class="select-timer">
      <button @click="setTime(1500)">25:00</button>
      <button @click="setTime(3600)">60:00</button>
      <button @click="setTime(5400)">90:00</button>
      <button @click="setTime(7200)">120:00</button>
    </div>
    <h1>Pomotime</h1>
    <h2>{{Timeleft}}</h2>
    <div v-if="play">
      <button @click="startInterval">{{status}}</button>
    </div>
    <div v-else>
      <button @click="stopInterval">{{status}}</button>
    </div>
     <audio ref="audio" loop id="audio">
        <source src="../assets/firstdate.mp3" type="audio/ogg">
           Your browser does not support the audio element.
    </audio> 
  </div>
</template>

<script>
export default {
  name: 'Timer',
  data(){
    return{
      status : 'Start',
      limitTime: 1500,
      passTime: 0,
      startTimer: null,
      play: true
    }
  },
  computed:{
    Timeleft(){
      const time = this.limitTime - this.passTime;
      const min = Math.floor(time/60);
      let sec = time % 60;
      // make sure the second has 2 length value
      if(sec < 10){
        sec = `0${sec}`;
      }
      return min + ':' + sec;
    }
  },
  methods: {
    startInterval(){
      this.play = false;
      this.status = 'Stop';
        this.startTimer = setInterval(() => {
          this.passTime ++;
          document.querySelector('#audio').play();
          if(this.passTime == this.limitTime){
            clearInterval(this.startTimer);
              document.querySelector('#audio').pause();
              this.status = 'Time end';
              document.querySelector('#audio').currentTime = 0;
          }
        },1000);
      if(this.passTime == this.limitTime){
        this.passTime = 0;
        clearInterval(this.startTimer);
        this.status = 'Start';
        document.querySelector('#audio').currentTime = 0;
      }
    },
    setTime: function (params){
      this.limitTime = params;
      this.clearAll();
    },
    stopInterval(){
      this.status = 'Start';
      this.play = true;
      clearInterval(this.startTimer);
      document.querySelector('#audio').pause();
    },
    clearAll(){
      this.status = 'Prepare';
      this.passTime = 0;
      clearInterval(this.startTimer);
      document.querySelector('#audio').pause();
      // reset audio
      document.querySelector('#audio').currentTime = 0;
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  button{
    padding: 4px 8px;
    font-size: 1.2em;
    color: white;
    background-color: rgb(50, 219, 135);
    border: 1px solid rgb(86, 196, 86);
  }
  .select-timer button{
    margin: 0 4px;
  }
  button:hover{
    background-color: rgb(71, 194, 71);
  }
</style>
