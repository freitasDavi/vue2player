<template>
  <div id="app">
    <header>
      <h1>My music</h1>
    </header>
    <main>
      <section class="player">
        <h2 class="song-title">{{current.title}} - <span>{{ current.artist }}</span> </h2>
        <div class="controls">
          <button class="prev" @click="previous">Prev</button>
          <button class="play" v-show="!isPlaying" @click="play">Play</button>
          <button class="pause" v-show="isPlaying" @click="pause">Pause</button>
          <button class="next" @click="next">Next</button>
        </div>
      </section>
      <section class="playlist">
        <h3>Playlist</h3>
        <button v-for="song in songs" :key="song.title" @click="play(song)" :class="(song.src === current.src) ? 'song playing' : 'song'">{{song.title}}</button>
      </section>
    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data: function () {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
        {
          title: 'Grateful',
          artist: 'Neffex',
          src: require("./assets/neffex-grateful.mp3")
        },
        {
          title: 'Invincible',
          artist: 'Deaf Kev',
          src: require("./assets/src_assets_deaf-kev-invincible.mp3")
        }
      ],
      player: new Audio()
    }
  },
  methods: {
    play(song) {
      if(typeof song.src != "undefined") {
        this.current = song;

        this.player.src = this.current.src;
      }

      this.player.play();
      this.player.addEventListener('ended', function () {
        this.index++;

        if(this.index < 0) {
          this.index = this.songs.length - 1;
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

    previous() {
      this.index--;
      if(this.index < 0) {
        this.index = this.songs.length - 1;
      }

      this.current = this.songs[this.index];

      this.play(this.current);
    },

    next() {
      this.index++;
      if(this.index > this.songs.length - 1) {
        this.index = 0;
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
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  body {
    font-family: sans-serif;
  }

  header {
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 15px;
    background-color: #212121;
    color: #Fff;
  }

  main {
    width: 100%;
    max-width: 768px;
    margin: 0 auto;
    padding: 25px;
  }

  .song-title {
    color: #53565A;
    font-size: 32px;
    font-weight: 700;
    text-transform: uppercase;
    text-align: center;
  }

  .song-title span {
    font-weight: 400;
    font-style: italic;
  }

  .controls {
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 30px 15px;
  }

  button {
    appearance: none;
    background: none;
    border: none;
    outline: none;
    cursor: pointer;
  }

  button:hover {
    opacity: 0.8;
  }

  .play, .pause {
    font-size: 20px;
    font-weight: 700;

    padding: 15px 25px;
    margin: 0px 15px;
    border-radius: 8px;

    color: #fff;
    background: #cc2e5d;

    transition: opacity .5s;
  }

  .next, .prev {
    font-size: 16px;
    font-weight: 700;

    padding: 10px 20px;
    margin: 0px 15px;
    border-radius: 6px;

    color: #fff;
    background: #ff5858;

    transition: opacity .5s;
  }

  .playlist {
    padding: 0 30px;
  }

  .playlist h3 {
    color: #212121;
    font-weight: 400;
    font-size: 28px;
    margin-bottom: 30px;
    text-align: center;
  }

  .playlist, .song {
    display: block;
    width: 100%;
    padding: 15px;
    font-size: 20px;
    font-weight: 700;
    cursor: pointer;
  }

  .playlist .song:hover {
    color: #ff5858;
  }

  .playlist .song.playing {
    color: #fff;
    background-image: linear-gradient(to right, #cc2e5d, #ff5858);
  }

</style>
