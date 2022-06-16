<template>
  <div id="app">
    <header>
      <h1>My Music</h1>
    </header>
    <main>
      <section class="player">
        <h2 class="song-title">{{ current.title }} - <span>{{ current.artist }}</span></h2>
        <div class="controls">
          <button class="prev" @click="prev">Prev</button>
          <button v-if="!isPlaying" class="play" @click="play">Play</button>
          <button v-else class="pause" @click="pause">Pause</button>
          <button class="next" @click="next">Next</button>
        </div>
      </section>
      <section class="playlist">
        <h3>The Playlist</h3>
        <button v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src == current.src) ? 'song-playing' : 'song'">
          {{ song.title }} - {{ song.artist }}
        </button>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      current: {},
      index: 1,
      isPlaying: false,
      songs: [
        {
          title: "Sky High",
          artist: "Elektronomia",
          src: require('./assets/elektronomia_sky_high.mp3')
        },
        {
          title: "Eclipse",
          artist: "Jim Yosef",
          src: require('./assets/jim_yosef_eclipse.mp3')
        },
        {
          title: "Firefly",
          artist: "Jim Yosef",
          src: require('./assets/jim_yosef_firefly.mp3')
        },
        {
          title: "Fresh Time",
          artist: "Roa",
          src: require('./assets/roa_fresh_time.mp3')
        },
      ],
      player: new Audio()
    }
  },

  methods: {
    play(song) {
      if(typeof song.src != "undefined") {
        this.isPlaying = true;
        this.current = song;
        this.player.src = this.current.src;
      }
      this.player.play();

      this.player.addEventListener('ended', function() {
        this.index++;
        if(this.index > this.songs.length - 1) {
          this.index = 0;
        }
        this.current = this.songs[this.index];
        this.play(this.current);
      }.bind(this));

      this.isPlaying = true;
    },

    pause() {
      this.player.pause();
      this.isPlaying = false;
    },

    next() {
      this.index++;
      if(this.index > this.songs.length - 1) {
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },

    prev() {
      this.index--;
      if(this.index < 0) {
        this.index = this.songs.length - 1;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    }
  },

  created() {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
    this.player.play();
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
}

header {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
  color: #FFF;
  background-color: #404040;
}

main {
  width: 100%;
  max-width: 750px;
  margin: 0 auto;
  padding: 15px;
}

.song-title {
  color: green;
  font-size: 30px;
  font-weight: 700;
  text-align: center;
}

.song-title span {
  color: rgba(70, 70, 70, .7);
}

.controls {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 30px 15px;
}

button {
  appearance: none;
  border: none;
  background: none;
  outline: none;
  cursor: pointer;
}

button:hover {
  opacity: 0.8;
}

.play {
  font-size: 20px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0 15px;
  color: #FFF;
  background-color: rgb(0, 128, 0);
  border-radius: 20%;
}

.pause {
  font-size: 20px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0 15px;
  color: #FFF;
  background-color: rgb(200, 128, 0);
  border-radius: 20%;
}

.prev, .next {
  font-size: 15px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0 15px;
  color: #FFF;
  background-color: rgb(0, 128, 0);
  border-radius: 40%;
}

.playlist h3 {
  text-align: center;
  font-size: 25px;
  text-decoration: dotted underline;
  margin: 15px;
}

.playlist .song {
  text-align: center;
  display: block;
  font-size: 120%;
  width: 70%;
  height: fit-content;
  padding: 15px;
  margin: auto;
  cursor: pointer;
}

.playlist .song-playing {
  text-align: center;
  color: white;
  display: block;
  font-size: 120%;
  width: 70%;
  height: fit-content;
  padding: 15px;
  margin: auto;
  cursor: pointer;
  background-image: linear-gradient(to right, rgb(0, 128, 0), rgb(150, 250, 250));
}
</style>




