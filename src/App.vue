 

<template>
  <div class="screen">
    <copy-right-screen />
    <main-screen v-if="statusMatch == 'default'" @onStart="onHandelBeforeStart($event)" />
    <interact-screen
      v-if="statusMatch == 'match'"
      :cardsContext="settings.cardsContext"
      @onFinish="onGetResult"
      :columns="columns"
      @onBackToHome="onStartGame"
    />
    <game-result v-if="statusMatch == 'result'" :timer="timer" @onStartGame="onStartGame" />
  </div>
</template>

<script lang="ts">
import MainScreen from './components/MainScreen.vue'
import InteractScreen from './components/InteractScreen.vue'

import { shuffled } from './utils/array'
import GameResult from './components/GameResult.vue'
import CopyRightScreen from './components/CopyRightScreen.vue'
export default {
  components: { MainScreen, InteractScreen, GameResult, CopyRightScreen },
  data() {
    return {
      settings: {
        totalOfBlocks: 0,
        cardsContext: [],
        startedAt: null
      },
      timer: 0,
      columns: null,
      statusMatch: 'default'
    }
  },
  methods: {
    onHandelBeforeStart(config) {
      this.settings.totalOfBlocks = config.totalOfBlock
      const firstCards = Array.from(
        {
          length: this.settings.totalOfBlocks / 2
        },
        (_, i) => i + 1
      )
      const secondCards = [...firstCards]

      this.settings.cardsContext = shuffled(
        shuffled(shuffled(shuffled([...firstCards, ...secondCards])))
      )
      const numberColumns = Math.sqrt(config.totalOfBlock)
      let columns = ''
      for (let i = 0; i < numberColumns; i++) {
        columns += 'auto '
      }
      this.columns = columns

      this.settings.startedAt = new Date().getTime()

      this.statusMatch = 'match'
    },
    onGetResult() {
      this.timer = new Date().getTime() - this.settings.startedAt
      this.statusMatch = 'result'
    },
    onStartGame() {
      this.statusMatch = 'default'
    }
  }
}
</script>
<style scoped>
.screen {
  height: 100vh;

  top: 0;
  left: 0;
  z-index: 2;
  background-repeat: no-repeat;
  background-size: cover;
  background-image: url('https://wallpapers-clan.com/wp-content/uploads/2023/11/cute-pokemon-pikachu-rain-desktop-wallpaper-preview.jpg');
  background-color: var(--dark);
  color: var(--light);
}
</style>

