<template>
  <div class="container">
    <div>
      <h1 class="title">{{ currentChord }}</h1>
      <button @click="controlInterval()" class="button">
        <span v-if="!isRunning">Start</span> <span v-else>Stop</span>
      </button>
    </div>
  </div>
</template>

<script>
import { default as Chords } from '../plugins/chords'

export default {
  data() {
    return {
      isRunning: false,
      chords: Chords,
      tempo: 500,
      t: null,
      currentChord: null,
    }
  },
  methods: {
    computeChord: function () {
      return this.chords.root[
        Math.floor(Math.random() * this.chords.root.length)
      ]
    },
    controlInterval: function () {
      if (!this.isRunning) {
        this.isRunning = true
        this.t = setInterval(() => {
          this.currentChord = this.computeChord()
          console.log(this.currentChord)
        }, this.tempo)
      } else {
        this.isRunning = false
        clearInterval(this.t)
        this.t = null
      }
    },
  },
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
