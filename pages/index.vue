<template>
  <div class="container">
    <section class="section">
      <div class="display">
        <h1 class="chord has-text-weight-bold">
          <span class="chord__root">
            {{ currentChord.root }}
          </span>
          <span
            :class="
              'chord__alteration chord__alteration--' + currentChord.alteration
            "
          >
            {{ currentChord.alteration }}
          </span>
          <span class="chord__type">
            {{ currentChord.type }}
          </span>
        </h1>
      </div>
      <div class="control">
        <div class="field has-addons">
          <div class="control">
            <input
              class="input is-medium is-rounded"
              type="text"
              placeholder="Tempo"
              v-model="tempo"
              v-on:keyup="typeInput"
            />
          </div>
          <div class="control">
            <a
              class="button is-medium is-rounded"
              :class="isRunning ? 'is-link' : 'is-primary'"
              @click="controlInterval()"
              ><span v-if="!isRunning">Start</span> <span v-else>Stop</span></a
            >
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import { default as Chords } from '../plugins/chords'

export default {
  data() {
    return {
      isRunning: false,
      chords: Chords,
      tempo: 1500,
      t: null,
      currentChord: {
        root: '',
        alteration: '',
        type: '',
      },
    }
  },
  methods: {
    computeRoot: function () {
      return this.chords.root[
        Math.floor(Math.random() * this.chords.root.length)
      ]
    },
    computeAlteration: function () {
      return this.chords.alterations[
        Math.floor(Math.random() * this.chords.alterations.length)
      ]
    },
    computeType: function () {
      return this.chords.type[
        Math.floor(Math.random() * this.chords.type.length)
      ]
    },
    typeInput: function (e) {
      if (e.key === 'Enter' || e.keyCode === 13) {
        this.controlInterval()
      } else {
        this.stopChordMachine()
      }
    },
    stopChordMachine: function () {
      console.log('stop')
      this.isRunning = false
      clearInterval(this.t)
      this.t = null
    },
    controlInterval: function () {
      if (!this.isRunning) {
        this.isRunning = true
        this.t = setInterval(() => {
          this.currentChord.root = this.computeRoot()
          this.currentChord.alteration = this.computeAlteration()
          this.currentChord.type = this.computeType()
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

<style lang="scss">
.chord-machine {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  height: 100vh;
}
.display {
  margin: 6rem 0;
}
.chord {
  height: 200px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 8rem;

  &__root {
    color: #12151f;
  }
  &__alteration {
    color: #05f4b7;

    &--b {
      margin-top: 45px;
    }

    &--\# {
      margin-bottom: 45px;
    }
  }
  &__type {
    color: #371bb1;
  }
}

.control {
  display: flex;
  justify-content: center;
  align-items: center;
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
