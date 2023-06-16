<template>
  <div
    class="card"
    @click="onToggleFlipCard"
    :class="{ disabled: isDisable }"
    :style="{
      height: `${(750 - 16 * 4) / Math.sqrt(cardsContext.length) - 16}px`,
      width: `${
        (((750 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4
      }px`,
      perspective: `${
        ((((750 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4) * 2
      }px`,
    }"
  >
    <div class="card__inner" :class="{ 'is-flipped': isFlipped }">
      <div class="card__face card__face--front">
        <div
          class="card__content"
          :style="{
            'background-size': `${
              (((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) /4 /3}px 
              ${ (((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) /4 /3}px`,
          }"
        ></div>
      </div>
      <div class="card__face card__face--back">
        <div
          class="card__content"
          :style="{ backgroundImage: `url(${require('@/assets/' + img)})` }"
        ></div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    img: {
      type: String,
      required: true,
    },
    card: {
      type: [Array, String, Number, Object],
    },
    cardsContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  data() {
    return {
      isFlipped: false,
      isDisable: false,
    };
  },
  methods: {
    onToggleFlipCard() {
      if (this.isDisable) return false;
      this.isFlipped = !this.isFlipped;
      if (this.isFlipped) this.$emit("onFlip", this.card);
    },
    onDisable() {
      this.isDisable = true;
    },
    onFlipBackCard() {
      this.isFlipped = false;
    },
  },
};
</script>

<style scoped lang="css">
.card {
  display: inline-block;
  margin-right: 1rem;
  margin-bottom: 1rem;
}
.card__inner {
  width: 100%;
  height: 100%;
  transition: transform 1s;
  transform-style: preserve-3d;
  cursor: pointer;
  position: relative;
}
.disabled {
  cursor: default;
}
.is-flipped {
  transform: rotateY(180deg);
}
.card__face {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  border-radius: 1rem;
  overflow: hidden;
  padding: 1rem;
  box-shadow: 0 3px 10px 3px rgba(0, 0, 0, 0.2);
}
.card__face--front .card__content {
  background: url("../assets/images/icon_back.png") no-repeat center center;
  background-size: 40px 40px;
  height: 100%;
}
.card__face--back {
  transform: rotateY(-180deg);
  background: var(--light);
}
.card__face--back .card__content {
  background-position: center center;
  background-repeat: no-repeat;
  background-size: contain;
  height: 100%;
  width: 100%;
}
</style>