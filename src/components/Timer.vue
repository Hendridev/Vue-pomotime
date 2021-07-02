<template>
  <div>
    <img alt="Vue logo" src="/med/logo.svg">
    <h1>Pomotime</h1>
    <label for="cx">Sound off</label>
      <input v-model="enableSong" type="checkbox" name="cx" id="cx" placeholder="Sound off">
    <div>
      <select @change="songChange">
        <option v-for="song in songlist" :key="song.name" :value="song.value">{{song.name}}</option>
      </select>
    </div>
    <div class="select-timer">
      <button @click="setTime(1500)">25:00</button>
      <button @click="setTime(3600)">60:00</button>
      <button @click="setTime(5400)">90:00</button>
      <button @click="setTime(7200)">120:00</button>
    </div>
    <div>
      <p>current song : {{currentSong}}</p>
    </div>
    <div class="timewrapper">
      <div>
        <h2>{{Timeleft}}</h2>
      </div>
    </div>
    <div v-if="play">
      <button @click="startInterval">{{status}}</button>
    </div>
    <div v-else>
      <button @click="stopInterval">{{status}}</button>
    </div>
      <audio ref="audio" loop id="audio" :src="audioSrc"></audio>
  </div>
</template>

<script>
export default {
  name: 'Timer',
  data(){
    return{
      enableSong: false,
      status : 'Start',
      limitTime: 1500,
      passTime: 0,
      startTimer: null,
      play: true,
      currentSong: 'first date',
      audioSrc: '/med/firstdate.mp3',
      songlist: [
        {name: 'First date', value: '/med/firstdate.mp3'},
        {name: 'Ylang ylang', value: '/med/ylang.mp3'},
        {name: 'Relax', value: '/med/relax.mp3'},
        {name: 'Spirited away', value: '/med/spiritedaway.mp3'},
        {name: 'Shelter', value: '/med/shelter.mp3'},
        {name: 'Nandemonaiya', value: '/med/nandemonay.mp3'},
        {name: 'New World', value: '/med/wholenewworld.mp3'},
        {name: 'Summertime', value: '/med/summertime.mp3'},
        {name: 'Happier', value: '/med/happier.mp3'},
        {name: 'Halu', value: '/med/halu.mp3'},
      ]
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
    songChange(e){
      this.audioSrc = e.target.value; 
      this.currentSong = this.songlist[e.target.selectedIndex].name;
    },
    startInterval(){
      this.play = false;
      this.status = 'Stop';
        this.startTimer = setInterval(() => {
          this.passTime ++;
          if(this.enableSong == false){
            document.querySelector('#audio').play();
          }
          if(this.enableSong == true){
             document.querySelector('#audio').pause();
          }
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
  *{
    font-family: 'Poppins', sans-serif;
  }
  img{
    width: 80px;
  }
  button{
    padding: 4px 8px;
    font-size: 1em;
    color: white;
    background-color: rgb(50, 219, 135);
    border: 1px solid rgb(86, 196, 86);
    border-radius: 4px;
  }
  select{
    padding: 4px 6px;
    font-size: 0.9em;
    color: white;
    background-color: rgb(50, 219, 135);
    border: 1px solid rgb(86, 196, 86);
    margin: 8px 0 12px 0;
    border-radius: 4px;
  }
  .select-timer button{
    margin: 0 4px;
  }
  button:hover{
    background-color: rgb(71, 194, 71);
  }
  .timewrapper{
    padding: 4px;
    display: flex;
    justify-content: center;
  }
  .timewrapper div{
    padding: 4px 24px;
    border: 1px solid rgb(50, 219, 135);
    margin-bottom: 4px;
  }
</style>
