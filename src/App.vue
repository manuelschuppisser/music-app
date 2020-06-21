<template>
  <div id="app">
    <header>
      <h1>My Music</h1>
    </header>
    <main>
      <section class="player">
        <h2 class="song-title">
          {{ current.title }} -
          <span>{{ current.artist }}</span>
        </h2>
        <br />
        <div class="controls">
          <button class="prev" @click="prev">Prev</button>
          <button class="play" v-if="!isPlaying" @click="play">Play</button>
          <button class="pause" v-else @click="pause()">Pause</button>
          <button class="next" @click="next">Next</button>
        </div>
      </section>
      <h3>The Playlist</h3>
      <section class="playlist">
        <button
          id="song-wrapper"
          v-for="(song, index) in songs"
          :key="index"
          @click="togglePlay(index); play(song)"
          :class="song.src == current.src ? 'song playing' : 'song'"
        >
          <!-- CHANGEMENT:@click="togglePlay(index); play()"  -->
          {{ song.title }} - {{ song.artist }}
          <br />
          <img v-bind:src="song.img" />
          <!--<video source v-bind:src="song.src" width="320" height="240" controls /> -->
        </button>
        <!-- <a v-bind:href="url"></a> -->
        <!-- <np> -->
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name: "app",

  data() {
    return {
      geklikt: {},
      current: {},
      index: 0,
      currentPlayingIndex: 0,
      isPlaying: false,
      songs: [
        {
          title: "A Little Prayer",
          artist: "Aretha Franklin",
          src: require("./assets/prayer.mp3"),
          img: require("./assets/prayer.jpg")
        },
        {
          title: "Afrique Adieu",
          artist: "Michel Sardou",
          src: require("./assets/Afrique-adieu.mp3"),
          img: require("./assets/Michel-Sardou.jpg")
        },
        {
          title: "Boba Fet",
          artist: "TrucMuch",
          src: require("./assets/Boba-Fett.mp3"),
          img: require("./assets/boba-fet-img.jpg")
        },
        {
          title: "I Can Walk",
          artist: "dreamenglish",
          src: require("./assets/I Can Walk_Tb8lZaIZw_M.mp3"),
          img: require("./assets/dreamenglish.jpg")
        },

        {
          title: "Invincible",
          artist: "Deaf Kev",
          src: require("./assets/deaf-kev-invincible.mp3"),
          img: require("./assets/oeil.jpg")
        },
        {
          title: "Visitor",
          artist: "Koto",
          src: require("./assets/KOTO-VISITOR.mp4"),
          img: require("./assets/koto-img.jpg")
        }
      ],
      player: new Audio()
    };
  },
  methods: {
    togglePlay(songIndex) {
      if (this.currentPlayingIndex == songIndex) {
        if (this.isPlaying) {
          this.player.pause();
          this.isPlaying = false;
        } else {
          this.player.play();
          this.isPlaying = true;
        }
      } else {
        this.player.src = this.songs[songIndex].src;
        this.currentPlayingIndex = songIndex;
        this.player.play();
        this.isPlaying = true;
      }
    },

    play(song) {
      if (typeof song.src != "undefined") {
        this.current = song;
        this.player.src = this.current.src;
      }

      this.player.play();

      this.player.addEventListener(
        "ended",
        function() {
          this.index++;
          if (this.index > this.songs.length - 1) {
            this.index = 0;
          }

          this.current = this.songs[this.index];
          this.play(this.current);
        }.bind(this)
      );
      this.isPlaying = true;
    },
    pause() {
      this.player.pause();
      this.isPlaying = false;
    },
    next() {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }

      this.current = this.songs[this.index];
      this.play(this.current);
    },
    prev() {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }

      this.current = this.songs[this.index];
      this.play(this.current);
    }
  },
  created() {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  }
};
</script>


<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: sans-serif;
  background-color: #fff;
}

header {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
  background-color: #212121;
  color: #fff;
}
main {
  width: 100%;
  margin: 0 auto;
  padding: 25px;
}
.dynamic-song {
  width: 15vw;
  height: 3vw;
}
.song-title {
  color: #53565a;

  letter-spacing: 0.9vw;
  font-size: 17px !important;

  background-color: rgb(255, 204, 204);
  border: 2px;
  border-color: black;
  border-radius: 10px;
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

.play,
.pause {
  font-size: 20px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0px 15px;
  border-radius: 50%;
  color: #fff;
  background-color: #cc2e5d;
}

.next {
  font-size: 16px;
  font-weight: 100;
  padding: 10px 20px;
  margin: 0px 15px;
  border-radius: 0% 50% 50% 0%;
  color: #fff;
  background-color: #ff5858;
}
.prev {
  font-size: 16px;
  font-weight: 100;
  padding: 10px 20px;
  margin: 0px 15px;
  border-radius: 50% 0% 0% 50%;
  color: #fff;
  background-color: #ff5858;
}
#song-wrapper {
  min-width: 25vw;
}
img {
  border-radius: 50%;
  border: 30px;
  width: 300px;
  height: 300px;
  background: none;
}
video {
  margin-top: -150px;
  display: flex;
  justify-content: center;
  background: none;
  position: relative;
  z-index: 10;
}

#src {
  border-radius: 50%;
  border: 30px;
  width: 300px;
  height: 300px;
  background: none;
}
h3 {
  margin-top: 12px;
  font-style :italic, bold !important;
  font-family: sans-serif;
  letter-spacing: 0.2vw;
  font-weight: 10 bolder;
  font-size: 35px !important ;
  color: #202020 ;
}

.playlist {
  width: 100%;
  display: flex;
  justify-content: space-around;
  flex-wrap: wrap;
}
h3 {
  color: #202020;
  font-size: 32px;
  font-weight: 400px;
  margin-bottom: 30px;
  text-align: center;
  border-radius: 10px;
  text-decoration: underline rgb(255, 204, 204);
}
.playlist .song {
  display: block;
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
  border-radius: 1vw;

}
</style>

