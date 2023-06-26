<template>
  <div class="screen">
    <div
      class="screen__inner"
      :style="{
        width: `${
          ((((750 - 16 * 4) / Math.sqrt(cardContext.length) - 16) * 3) / 4 +
            16) *
          Math.sqrt(cardContext.length)
        }px`,
      }"
    >
      <card-flip
        v-for="(card, index) in cardContext"
        :key="index"
        :ref="`card-${index}`"
        :imgBackFaceUrl="`images/${card}.png`"
        :card="{ index, value: card }"
        :cardContext="cardContext"
        @onFlip="checkRule($event)"
      />
    </div>
  </div>
</template>

<script>
import CardFlip from "./CardFlip.vue";
export default {
  props: {
    cardContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  components: {
    CardFlip,
  },
  data() {
    return {
      rule: [],
    };
  },
  methods: {
    checkRule(card) {
      if (this.rule.length === 2) return false;

      this.rule.push(card);

      if (this.rule.length === 2 && this.rule[0].value === this.rule[1].value) {
        console.log("dung");
        this.$refs[`card-${this.rule[0].index}`][0].onEnable();
        this.$refs[`card-${this.rule[1].index}`][0].onEnable();

        this.rule = [];
        //check win
        const disable = document.querySelectorAll(".screen .card.disable");
        if (disable && disable.length === this.cardContext.length - 2) {
          setTimeout(() => {
            this.$emit("onFinish");
          }, 920);
        }
      } else if (
        this.rule.length === 2 &&
        this.rule[0].value !== this.rule[1].value
      ) {
        setTimeout(() => {
          //dong card khi sai
          this.$refs[`card-${this.rule[0].index}`][0].onFlipBackCard();
          this.$refs[`card-${this.rule[1].index}`][0].onFlipBackCard();
          // reset mang rule
          this.rule = [];
        }, 800);
      } else {
        return false;
      }
    },
  },
};
</script>

<style lang="css" scoped>
.screen {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  background-color: var(--dark);
  color: var(--light);
}
.screen__inner {
  display: flex;
  flex-wrap: wrap;
  margin: 2rem auto;
}
</style>
