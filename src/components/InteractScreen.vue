<template>
  <div class="screen">
    <div
      class="scrren__inner"
      :style="{
        width: `${
          ((((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4 +
            16) *
          Math.sqrt(cardsContext.length)
        }px`,
      }"
    >
      <cart-flip
        v-for="(card, index) in cardsContext"
        :key="index"
        :ref="`card-${index}`"
        :imgBackFaceUrl="`images/${card}.png`"
        :card="{ index, value: card }"
        @onFlip="checkRule($event)"
        :cardsContext="cardsContext"
        :isFlipping="isFlipping"
      ></cart-flip>
    </div>
  </div>
</template>
<script>
import CartFlip from "./Card.vue";
export default {
  props: {
    cardsContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  components: {
    CartFlip,
  },
  data() {
    return {
      rules: [],
      isFlipping: false,
    };
  },
  methods: {
    checkRule(card) {
      // console.log(card);
      if (this.rules.length === 2) return false;
      this.rules.push(card);
      if (
        this.rules.length === 2 &&
        this.rules[0].value === this.rules[1].value
      ) {
        console.log("right");
        // add class disabled to component card
        this.$refs[`card-${this.rules[0].index}`][0].onEnableDisableMode();
        this.$refs[`card-${this.rules[1].index}`][0].onEnableDisableMode();
        //reset rules
        this.rules = [];

        const disabledElements = document.querySelectorAll(
          ".screen .card.disabled"
        );
        if (
          disabledElements &&
          disabledElements.length === this.cardsContext.length - 2
        ) {
          setTimeout(() => {
            this.$emit("onFinish");
          }, 920);
        }
      } else if (
        this.rules.length === 2 &&
        this.rules[0].value !== this.rules[1].value
      ) {
        this.isFlipping = true;
        console.log("error");
        // close two card
        setTimeout(() => {
          this.$refs[`card-${this.rules[0].index}`][0].onFlipBackCard();
          this.$refs[`card-${this.rules[1].index}`][0].onFlipBackCard();
          // reset rules = []
          this.rules = [];
          // reset isFlipping = []
          this.isFlipping = false;
        }, 800);
      } else return false;
    },
  },
};
</script>
<style lang="css">
.screen {
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  z-index: 2;
  background-color: var(--dark);
  color: var(--light);
}
.scrren__inner {
  display: flex;
  flex-wrap: wrap;
  margin: 2rem auto;
}
</style>
