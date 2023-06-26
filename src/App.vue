<template>
  <main-screen
    v-if="statusMath === 'default'"
    @onStart="onHandleBeforeStart($event)"
  />
  <interact-screen
    v-if="statusMath === 'match'"
    :cardContext="setting.cardContext"
    @onFinish="onGetResult"
  />
  <result-screen
    v-if="statusMath === 'result'"
    :timer="timer"
    @onStartAgain="statusMath = 'default'"
  />
  <copy-right />
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import ResultScreen from "./components/ResultScreen.vue";
import CopyRight from "./components/CopyRight.vue";
import { shuffled } from "./utils/Array";
export default {
  name: "App",
  data() {
    return {
      setting: {
        totalOfBlocks: 0,
        cardContext: [],
        startedAt: null,
      },
      statusMath: "default",
      timer: 0,
    };
  },
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
    CopyRight,
  },
  methods: {
    onHandleBeforeStart(config) {
      console.log("start", config);
      this.setting.totalOfBlocks = config.totalOfBlocks;

      // tao mang
      const firstCards = Array.from(
        { length: this.setting.totalOfBlocks / 2 },
        (_, i) => i + 1
      );

      const secondCard = [...firstCards];

      // mix 2 mang
      const cards = [...firstCards, ...secondCard];
      this.setting.cardContext = shuffled(
        shuffled(shuffled(shuffled(shuffled(cards))))
      );
      console.log(this.setting.cardContext);
      // start time
      this.setting.startedAt = new Date().getTime();
      //data readty
      this.statusMath = "match";
    },
    onGetResult() {
      this.timer = new Date().getTime() - this.setting.startedAt;
      this.statusMath = "result";
    },
  },
};
</script>
