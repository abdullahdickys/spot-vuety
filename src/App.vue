<template>
  <div id="app">
    <header>
		<h1>My Music</h1>
    </header>
    <main>
	<section class="player">
		<br>
		<h2 class="song-title">{{ current.title }} - <span>{{ current.artist }}</span></h2>
		<div class="controls">
			<button class="prev" @click="prev">Prev</button>
			<button class="play" v-if="!isPlaying" @click="play">Play</button>
			<button class="pause" v-else @click="pause">Pause</button>
			<button class="next" @click="next">Next</button>
		</div>
	</section>
	<section class="playlist">
		<h3>The Playlist</h3>
		<button v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src == current.src) ? 'song playing' : 'song'">{{ song.title }} - {{ song.artist }}</button>
	</section>
    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data () {
	return {
	current: {
		title: 'SONG TITLE'
	},
	songs: [
		{
			title: 'Wow',
			artist: 'Post Malone',
			src: require('./assets/Post Malone - Wow.mp3')
		},
		{
			title: 'One call away',
			artist: 'Charlie Puth',
			src: require('./assets/One Call Away - Charlie Puth.mp3')
		}
	],
	player: new Audio()
	}
  },
  methods: {
	play (song) {
		if (typeof song.src != "undefined") {
			this.current = song;

			this.player.src = this.current.src;
		}
		this.player.play();
		this.player.addEventListener('ended', function(){
			this.index++;
			if (this.index > this.songs.length - 1) {
				this.index = 0;
			}
			this.current = this.songs[this.index];
			this.play(this.current);
		}.bind(this));
		this.isPlaying = true;
	},
	pause () {
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
	prev () {
		this.index--;
		if (this.index < 0) {
			this.index = this.songs.length - 1;
		}
		this.current = this.songs[this.index];
		this.play(this.current);
	}
  },
  create () {
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
	color: #FFF;
}
main {
	width: 0;
	min-width: 768px;
	margin: 0 auto;
	padding: 25px;
}
.song-title {
	color: #53565a;
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
	padding: 30px 15px;
}
</style>
