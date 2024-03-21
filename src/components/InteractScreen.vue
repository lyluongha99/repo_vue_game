<template>
  <div class="screen">
    <div
      class="screen__inner"
      :style="{
        width: `${
          ((((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4 +
            16) *
          Math.sqrt(cardsContext.length)
        }px`,
      }"
    >
      <card-flip
        v-for="(card, index) in cardsContext"
        :key="index"
        :ref="`card-${index}`"
        :imgBackFaceUrl="`img/${card}.png`"
        :card="{ index, value: card }"
        @onFlip="checkrule($event)"
        :cardsContext="cardsContext"
      />
    </div>
  </div>
</template>
<script>
import CardFlip from "./CardPlay.vue";
export default {
  name: "InteractScreen",
  props: {
    cardsContext: {
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
      rules: [],
    };
  },
  methods: {
    checkrule(card) {
      if (this.rules.length === 2) return false;

      this.rules.push(card);

      if (
        this.rules.length === 2 &&
        this.rules[0].value === this.rules[1].value
      ) {
        console.log("right...");
        // addclass "disabled" to component card
        this.$refs[`card-${this.rules[0].index}`][0].onEnabledDisableMode();

        this.$refs[`card-${this.rules[1].index}`][0].onEnabledDisableMode();
        this.rules = [];
        const disabledElements = document.querySelectorAll(
          ".screen .card.isDisabled"
        );
        console.log(disabledElements.length);
        console.log(this.cardsContext.length - 2);

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
        setTimeout(() => {
          console.log("wrong...");
          //close two card
          this.$refs[`card-${this.rules[0].index}`][0].onFlipBackCard();

          this.$refs[`card-${this.rules[1].index}`][0].onFlipBackCard();
          //reset rule to []
          this.rules = [];
        }, 800);
      } else return false;
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
  width: 424px;
  display: flex;
  flex-wrap: wrap;
  margin: 2rem auto;
}
</style>
