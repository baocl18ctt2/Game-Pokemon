<template>
  <div class="screen">
    <div
      class="screen__inner"
      :style="renderWidthCardsScreen()"
    >
      <!-- Thành phần trong component Card.vue -->
      <card-flip
        v-for="(card, index) in cards"
        :key='index'
        :imgBackFaceUrl="`images/${card}.png`"
        :card="{index, value: card}"
        @onFlip="checkRule($event)"
        ref="cards"
        :cardContext='cards'
        @getSizeCard="getSizeCard($event)"
      />
    </div>
  </div>

</template>

<script>
import CardFlip from "../views/Card.vue";
export default {
  data() {
    return {
      // Chửa phần tử sẽ mở
      rules: [],
      size: {
        width: 0,
      },
    };
  },
  props: {
    cards: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  components: {
    CardFlip,
  },
  methods: {
    checkRule(config) {
      console.log("cards", this.cards.length);
      console.log(config);
      // Nếu số lượng card đang lật == 2 thì không làm gì hết.
      // Ngược lại thì sẽ push từng card vào trong rules.
      if (this.rules.length == 0 || this.rules.length == 1) {
        this.rules.push(config);
      }
      if (this.rules.length == 2) {
        // Trường hợp lật 2 ảnh giống nhau
        if (this.rules[0].value == this.rules[1].value) {
          // Lật 2 ảnh đó lại
          let a0 = this.rules[0].index;
          let a1 = this.rules[1].index;
          setTimeout(() => {
            this.$refs.cards[a0].onNotFlippedMode();
            this.$refs.cards[a1].onNotFlippedMode();
          }, 800);
          // Xóa mảng rules
          this.rules = [];
          // số lượng phần tử đang là disable
          const disabledElement = document.querySelectorAll(
            ".screen .card.disabled"
          );
          // Nếu số lượng phần tử disabled = totalOf Block
          if (
            disabledElement &&
            disabledElement.length == this.cards.length - 2
          ) {
            setTimeout(() => {
              this.$emit("onFinishGame");
            }, 1000);
          }
        }
        // Trường hợp lật 2 ảnh khác nhau
        else {
          // Lật 2 ảnh đó lại
          let a0 = this.rules[0].index;
          let a1 = this.rules[1].index;
          setTimeout(() => {
            this.$refs.cards[a0].onFlipBackCard();
            this.$refs.cards[a1].onFlipBackCard();
          }, 800);
          // Xóa mảng rules
          this.rules = [];
        }
      }
    },
    getSizeCard(size) {
      this.size.width = size.width;
    },
    renderWidthCardsScreen() {
      let numRow = Math.sqrt(parseInt(this.cards.length));
      console.log("numRow", numRow);
      console.log("width", this.size.width);
      let widthCards = (parseFloat(this.size.width) + 16) * numRow;
      return { width: `${widthCards}px` };
    },
  },
};
</script>

<style scoped>
.screen {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  background-color: #000;
  color: #fff;
}
.screen__inner {
  /* width: 424px; */
  display: flex;
  flex-wrap: wrap;
  margin: 2rem auto;
}
</style>