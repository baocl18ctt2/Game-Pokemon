<template>
  <div
    class="card"
    :class="{'disabled': isDisabled}"
    :style="handleWidthHeightCard()"
  >
    <div
      class="card__inner"
      :class="{'is-flipped': isFlipped}"
      @click='onToggleFlipCard'
    >
      <!-- Phần lật ảnh trước -->
      <div class="card__face card__face--front">
        <div
          class="card__content"
          :style="sizeCardContent()"
        ></div>
      </div>
      <!-- Phần lật ảnh sau -->
      <div class="card__face card__face--back">
        <div
          class="card__content"
          :style="{'background-image': `url(${require('@/assets/'+ imgBackFaceUrl)})`}"
        ></div>
      </div>

    </div>
  </div>
</template>


<script>
export default {
  data() {
    return {
      //lật 1 ô
      isFlipped: false,
      isDisabled: false,
    };
  },
  props: {
    imgBackFaceUrl: {
      type: String,
      require: true,
    },
    card: {
      type: [String, Number, Array, Object],
    },
    flipBack: {
      type: [String, Number, Array, Object],
    },
    cardContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },

  methods: {
    onToggleFlipCard() {
      // Kiểm tra có tồn tại class disabled thì ko lật ảnh
      if (this.isDisabled) {
        return;
      }
      console.log("a", this.card);
      console.log("b", Math.sqrt(this.cardContext.length));
      this.isFlipped = !this.isFlipped;
      // Nếu card đang mở thì ...
      if (this.isFlipped) {
        this.$emit("onFlip", this.card);
      }
    },
    onFlipBackCard() {
      this.isFlipped = false;
    },
    onNotFlippedMode() {
      this.isDisabled = true;
    },
    handleWidthHeightCard() {
      let heightCard = (860 - 16 * 4) / Math.sqrt(this.cardContext.length) - 16;
      let widthCard =
        (((920 - 16 * 4) / Math.sqrt(this.cardContext.length) - 16) * 3) / 4;
      const obj = {
        width: `${widthCard}px`,
        height: `${heightCard}px`,
      };
      this.$emit("getSizeCard", obj);
      return obj;
    },
    sizeCardContent() {
      let width =
        (((920 - 16 * 4) / Math.sqrt(this.cardContext.length) - 16) * 3) / 12;
      return { "background-size": `${width}px ${width}px` };
    },
  },
};
</script>


<style lang="css" scoped>
.card {
  display: inline-block;
  margin-right: 1rem;
  margin-bottom: 1rem;
  /* width: 90px;
  height: 120px; */
  /* background-color: red; */
}
.card__inner {
  width: 100%;
  height: 100%;
  transition: transform 1s;
  transform-style: preserve-3d;
  cursor: pointer;
  position: relative;
}
.card.disabled .card__inner {
  cursor: default;
}
.card__inner.is-flipped {
  transform: rotateY(-180deg);
}
.card__face {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  overflow: hidden;
  border-radius: 1rem;
  padding: 1rem;
  box-shadow: 0 3px 18px 3px rgba(0, 0, 0, 0.2);
}
.card__face--front .card__content {
  background: url("../assets/images/icon_back.png") no-repeat center center;
  /* background-size: 40px 60px; */
  height: 100%;
  width: 100%;
}
.card__face--back {
  background-color: var(--light);
  transform: rotateY(-180deg);
}
.card__face--back .card__content {
  background-position: center center;
  background-repeat: no-repeat;
  background-size: contain;
  height: 100%;
  width: 100%;
}
</style>