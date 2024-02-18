<template>
  <div class="screen">
    <div class="nav">
      <h1>Game Play</h1>
      <h1 class="nav__home" @click="onBackToHome">Home</h1>
    </div>
    <div class="screen__card" :style="{ gridTemplateColumns: columns }">
      <card-flip
        v-for="(card, index) in cardsContext"
        :key="index"
        :ref="`card-${index}`"
        :imageUrl="`images/${card}.png`"
        :card="{ index, value: card }"
        @onFlip="checkRule($event)"
        :rules="rules"
      />
    </div>
  </div>
</template>
<script>
import CardFlip from './CardFlip.vue'
export default {
  props: {
    columns: {
      type: String
    },
    cardsContext: {
      type: Array,
      default: function () {
        return []
      }
    }
  },
  data() {
    return {
      rules: []
    }
  },
  methods: {
    checkRule(rule) {
      if (this.rules.length === 2) {
        this.rule = []
        return
      }

      this.rules.push(rule)
      if (this.rules.length === 2 && this.rules[0].value === this.rules[1].value) {
        this.$refs[`card-${this.rules[0].index}`][0].onFlipDisableCard()
        this.$refs[`card-${this.rules[1].index}`][0].onFlipDisableCard()
        this.rules = []
        const cardDisabledElement = document.querySelectorAll('.card.disable .card__inner')
        if (cardDisabledElement && cardDisabledElement.length === this.cardsContext.length - 2) {
          setTimeout(() => {
            this.$emit('onFinish')
          }, 800)
        }
      } else if (this.rules.length === 2 && this.rules[0].value !== this.rules[1].value) {
        setTimeout(() => {
          this.$refs[`card-${this.rules[0].index}`][0].onFlipBackCard()
          this.$refs[`card-${this.rules[1].index}`][0].onFlipBackCard()

          this.rules = []
        }, 800)
      } else {
        return false
      }
    },
    onBackToHome() {
      this.$emit('onBackToHome')
    }
  },
  components: {
    CardFlip
  }
}
</script>
<style scoped>
.screen {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  background-repeat: no-repeat;
  background-size: cover;
  background-image: url('https://wallpapers-clan.com/wp-content/uploads/2023/11/cute-pokemon-pikachu-rain-desktop-wallpaper-preview.jpg');
  background-color: var(--dark);
  color: var(--light);
}
.screen__card {
  display: grid;
  justify-content: center;
}
.nav {
  display: flex;
  justify-items: center;
  justify-content: space-between;
  padding: 20px 20px;
}
.nav .nav__home {
  cursor: pointer;
}
.nav .nav__home:hover {
  color: aquamarine;
}
</style>

