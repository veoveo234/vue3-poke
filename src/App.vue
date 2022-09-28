<template>
  <main-screen
    v-if="statusMatch === 'defalut'"
    @onStart="onHanldeBeforeStart($event)"
  ></main-screen>
  <interact-screen
    v-if="statusMatch === 'match'"
    :cardsContext="settings.cardsContext"
    @onFinish="onGetResult"
  ></interact-screen>
  <result-screen
    v-if="statusMatch === 'result'"
    :timer="timer"
    @onStartAgain="onStartAgain"
  />
  <copy-right />
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import ResultScreen from "./components/ResultScreen.vue";
import CopyRight from "./components/CopyRightScreen.vue";
import { shuffled } from "./ultis/array.js";
export default {
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
    CopyRight,
  },
  data() {
    return {
      settings: {
        totalBlocks: 0,
        cardsContext: [],
        startedAt: null,
      },
      timer: 0,
      statusMatch: "defalut",
    };
  },
  methods: {
    onHanldeBeforeStart(config) {
      console.log("running before start", config);
      this.settings.totalBlocks = config.totalOfBlocks;
      const firstCard = Array.from(
        { length: this.settings.totalBlocks / 2 },
        (_, i) => i + 1
      );
      const secondsCards = [...firstCard];
      const cards = [...firstCard, ...secondsCards];
      this.settings.cardsContext = shuffled(
        shuffled(shuffled(shuffled(cards)))
      );
      this.startedAt = new Date().getTime();
      // data ready
      this.statusMatch = "match";
    },
    onGetResult() {
      // get timer
      this.timer = new Date().getTime() - this.settings.startedAt;
      // switchto result component
      this.statusMatch = "result";
    },
    onStartAgain() {
      this.statusMatch = "defalut";
    },
  },
};
</script>

<style></style>
