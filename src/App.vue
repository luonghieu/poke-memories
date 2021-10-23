<template>
  <main-screen
    v-if="statusMatch === 'default'"
    @onStart="onHandleBeforeStart"
  />
  <interact-screen
    v-if="statusMatch === 'match'"
    :cardsContext="settings.cardsContext"
    @onFinish="onGetResult"
  />
  <result-screen
    v-if="statusMatch === 'result'"
    :timer="timer"
    @onStartAgain="statusMatch = 'default'"
  />
  <copy-right-screen/>
</template>

<script>
import MainScreen from './components/MainScreen'
import InteractScreen from './components/InteractScreen'
import ResultScreen from './components/ResultScreen'
import CopyRightScreen from './components/CopyRightScreen'
import { shuffled } from './utils/array'

export default {
  name: "App",
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
    CopyRightScreen
  },
  data() {
    return {
      settings: {
        totalOfBlocks: 0,
        cardsContext: [],
        startedAt: null
      },
      timer: 0,
      statusMatch: "default",
    }
  },
  methods: {
    onHandleBeforeStart(configs) {
      this.settings.totalOfBlocks = configs.totalOfBlocks;

      const firstCards = Array.from(
        { length: this.settings.totalOfBlocks / 2 },
        (_, i) => i + 1
      );
      const secondCards = [...firstCards];
      const cards = [...firstCards, ...secondCards];

      this.settings.cardsContext = shuffled(shuffled(shuffled(cards)));

      this.settings.startedAt = new Date().getTime();
      //data ready
      this.statusMatch = 'match'
    },

    onGetResult() {
      this.statusMatch = "result";
      this.timer = new Date().getTime() - this.settings.startedAt;
    },
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
