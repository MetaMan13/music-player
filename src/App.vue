<template>
  <div id="app">
    <div>
      <audio @ended="nextSong" muted id="player">
        <source src="./assets/test.mp3">
        <source src="./assets/test2.mp3">
      </audio>

      <button @click="previousSong" class="bg-gray-900 hover:bg-green-600 text-white px-6 py-2 rounded-lg font-semibold text-base mt-2 ml-4">Previous</button>
      <button v-if="stopped" @click="playSong" class="bg-gray-900 hover:bg-green-600 text-white px-6 py-2 rounded-lg font-semibold text-base mt-2 ml-4">Play</button>
      <button v-if="playing" @click="stopSong" class="bg-gray-900 hover:bg-green-600 text-white px-6 py-2 rounded-lg font-semibold text-base mt-2 ml-4">Stop</button>
      <button @click="nextSong" class="bg-gray-900 hover:bg-green-600 text-white px-6 py-2 rounded-lg font-semibold text-base mt-2 ml-4">Next</button>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data(){
    return{
      player: null,
      playlist: null,
      playing: false,
      stopped: true,
      currentlyPlaying: null,
    }
  },
  mounted(){
    this.player = document.getElementById('player');
    this.playlist = Array.prototype.slice.call(this.player.querySelectorAll('source'))
    this.player.muted = false;
    this.currentlyPlaying = {
      index: 0,
      src: this.playlist[0].src
    };
  },
  methods: {
    playSong(){
      this.player.play();
      this.playing = true;
      this.stopped = false;
    },
    stopSong(){
      this.player.pause();
      this.playing = false;
      this.stopped = true;
    },
    previousSong(){
      this.stopSong()

      if((this.currentlyPlaying.index - 1) == -1)
      {
        this.currentlyPlaying.index = this.playlist.length - 1;
        this.currentlyPlaying.src = this.playlist[this.currentlyPlaying.index].src
      }else{
        this.currentlyPlaying.index = this.currentlyPlaying.index - 1
        this.currentlyPlaying.src = this.playlist[this.currentlyPlaying.index].src
      }

      this.player.src = this.currentlyPlaying.src
      this.playSong()
    },
    nextSong(){
      this.stopSong()

      if((this.playlist.length - 1) == this.currentlyPlaying.index)
      {
        this.currentlyPlaying.index = 0;
        this.currentlyPlaying.src = this.playlist[0].src
      }else{
        this.currentlyPlaying.index = this.currentlyPlaying.index + 1
        this.currentlyPlaying.src = this.playlist[this.currentlyPlaying.index].src
      }

      this.player.src = this.currentlyPlaying.src
      this.playSong()
    },
  }
}
</script>
