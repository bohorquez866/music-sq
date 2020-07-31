<template>
  <div id="app">
    <header>
      <h1>Music ZQ</h1>
    </header>

    <main>
      <section class="player">
        <h2 class="song-title">{{current.title}} - <span>{{current.artist}}</span> </h2>
      </section>
      
      <figure class="controls">
        <button class="prev" @click="prev">Prev</button>
        <button class="play" v-if="!isPlaying" @click="play">Play</button>
        <button class="pause" v-else @click="pause">Pause</button>
        <button class="next" @click="next">Next</button>
      </figure>
    </main>

    <section class="playlist">
      <h3>The playlist</h3>
      <button v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src == current.src ? 'song playing': 'song')">
        {{song.title}} - {{song.artist}}
      </button>
    </section>
   </div>
</template>

<script>

export default {
  name: 'App',
   data () {
     return {
current: {},
index: 0,
isPlaying: false,
songs:[
  {
  title: 'La flamme a lunettes',
  artist: 'Deonosys',
  src: require('./assets/la-flamme-a-lunettes.mp3')
  },
  {
  title: 'La flamme a lunettes 2',
  artist: 'Deonosys',
  src: require('./assets/la-flamme-a-lunettes 2.mp3')
  }
],
player: new Audio()
     }
   },
   methods: {

play(song) {

if (typeof song.src != "undefined") {
  this.current = song;
  this.player.src= this.current.src;
}

this.player.play();
this.player.addEventListener('ended', function() {
this.index++;
if(this.index > this.songs.length - 1) {
this.index= 0;
}
this.current = this.songs[this.index];
this.play(this.current);
}.bind(this))
this.isPlaying = true;

},
pause() {
  this.player.pause();
  this.isPlaying = false;
},
prev() {
this.index--;  

if(this.index < 0) {
this.index= this.songs.length -1;
}
this.current = this.songs[this.index];
this.play(this.current);
 
},

next() {
this.index++;  
if(this.index > this.songs.length - 1) {
this.index= 0;
}
this.current = this.songs[this.index];
this.play(this.current);

}
   },
   created () {
this.current = this.songs[this.index];
this.player.src = this.current.src;

   }
}
</script>



<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;

}
*, *::before, *::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
html {
  font-size: 62.5%;
}
header {
  display:flex;
justify-content: center;
align-items: center;
padding: 1.5rem;
background: #212121;
color: #fff;}

body {
  background-color: #3c5364;
}

main {
  width: 100%;
  max-width: 768px; 
  margin: 0 auto;
  padding: 2rem;
}

.song-title {
  color: #bec7d3;
  font-size: 3.2rem;
  font-weight: 700; 
  text-align: center ;
  text-transform: uppercase;
}
.song-title span {
  font-weight: normal;
  font-style: italic;
}
.controls {
  display: flex;
  justify-content: center;
  padding: 2rem 1rem;
  align-items: center;
}
button {
  color: #fff;
  appearance: none;
  background: #131127;
  border: 2px solid #1f1846 ;
  border-radius: 1rem;
  padding: 1rem;
  border: none;
  outline: none;
  cursor: pointer;
  transition: all .3s ease-in-out;
}
button:hover {
opacity: .9;
}

.play {
  font-size: 2rem;
  font-weight: 700;
  padding: 1.5rem 2.5rem;
  margin: 0 1.5rem;
  background-color:#1bb35a;
}
.prev, .next {
   font-size: 2rem;
  font-weight: 700;
  padding: 1rem 2rem;
  margin: 0 1.5rem;
  color: #ccc;
  background-color:#2b3380; 
}

.playlist {
  padding: 0 3rem;
}
.playlist h3 {
  color: #bec7d3;
font-size: 2.8rem;
margin-bottom: 2rem;
text-align: center;
}
.playlist .song {
  display: block;
  width: 70%;
  margin: 0 auto;
  padding: 1.5rem;
  margin-bottom: 1rem;
font-size: 2rem;
font-weight: 700;
}
.playlist .song.playing {
background: linear-gradient(to right,#2b3380, #5b68e0);
color: #ccc;
}
.playlist .song:hover {
  color: #1bb35a;
}
.playlist .song.playing:hover {
  color: #b1faf0;
}


@media  only screen and  (max-width: 768px) {
 
  .playlist {
  padding: 0 1.5rem;
}

.playlist .song {
    width: 100%;
}
}



</style>
