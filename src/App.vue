<template>
  <div>
    <main-screen
      v-if="statusMatch == 'default'"
      @onStart='onHandleBeforeStart($event)'
    />
    <interact-screen
      v-if="statusMatch == 'match'"
      :cards='settings.cardsConText'
      @onFinishGame='displayResultGame'
    />
    <result-screen
      v-if="statusMatch == 'result'"
      :timer="timer"
      :currentStatus="statusMatch"
      @onStartAgain="statusMatch = 'default'"
    />
    <!-- <register v-if="statusMatch == 'register'" /> -->
  </div>
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import ResultScreen from "./components/ResultScreen.vue";

import { shuffle } from "./utils/array";
export default {
  name: "App",
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
  },
  data() {
    return {
      settings: {
        totalOfBlocks: 0,
        // Mảng gộp của 2 mảng con
        cardsConText: [],
        // Thời gian bắt đầu
        startedAt: null,
      },
      statusMatch: "default",
      // timer: thời gian kết thúc game
      timer: 0,
      // Khai báo biến width
    };
  },
  methods: {
    onHandleBeforeStart(config) {
      let str = config.totalOfBlock;
      let numb = str.match(/(\d+)/);
      this.settings.totalOfBlocks = numb[0] * numb[1];
      // Chia làm 2 mảng
      // Khởi tạo mảng thứ nhất gồm (nxn)/2 phần tử
      const firstCards = Array.from(
        { length: this.settings.totalOfBlocks / 2 },
        (_, i) => i + 1
      );
      const secondCards = [...firstCards];
      const cards = [...firstCards, ...secondCards];

      // Gộp mảng ngẫu nhiên
      this.settings.cardsConText = shuffle(shuffle(shuffle(shuffle(cards))));
      this.settings.startedAt = new Date().getTime();
      console.log(this.settings.cardsConText);

      // Data khi render
      this.statusMatch = "match";
    },
    displayResultGame() {
      // Hiển thị thời gian kết thúc chương trình
      this.timer = new Date().getTime() - this.settings.startedAt;
      console.log("timer", this.timer);
      // Data khi render
      this.statusMatch = "result";
    },
  },
};
</script>

