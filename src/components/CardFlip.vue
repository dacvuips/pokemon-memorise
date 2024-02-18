<template>
  <div class="card" :class="{ disable: isDisable }">
    <div class="card__inner" :class="{ 'is-flipped': isFlipCard }" @click="onToggleFlipCard()">
      <div class="card__face card__face--front">
        <div class="card__content"></div>
      </div>
      <div class="card__face card__face--back">
        <div
          class="card__content"
          :style="{ backgroundImage: `url('../assets/${imageUrl}')` }"
        ></div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    imageUrl: {
      type: String,
      required: true
    },
    card: {
      type: [Number, Object]
    },
    rules: {
      type: [Object]
    }
  },
  data() {
    return {
      isDisable: false,
      isFlipCard: false
    }
  },
  methods: {
    onToggleFlipCard() {
      if (this.rules.length === 2) return
      if (this.isDisable) return
      this.isFlipCard = !this.isFlipCard
      if (this.isFlipCard) this.$emit('onFlip', this.card)
    },
    onFlipBackCard() {
      this.isFlipCard = false
    },
    onFlipDisableCard() {
      this.isDisable = true
    }
  }
}
</script>
<style lang="css" scoped>
.card {
  display: inline-block;
  margin-right: 1rem;
  margin-bottom: 1rem;
  width: 100px;
  height: 130px;
  perspective: 100px;
}
.card.disable .card__inner {
  cursor: default;
}
.card__inner {
  height: 100%;
  width: 100%;
  transition: transform 1s;
  transform-style: preserve-3d;
  cursor: pointer;
  position: relative;
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
  box-shadow: 0 3px 10px 3px rgba(0, 0, 0, 0.3);
}
.card__face--front {
  background-color: var(--dark);
}
.card__face--front .card__content {
  background: url('./assets/images/icon_back.png') no-repeat center center;
  background-size: 40px 40px;
  background-color: var(--dark);
  width: 100%;
  height: 100%;
}
.card__face--back {
  background-color: var(--light);
  transform: rotateY(-180deg);
}
.card__face--back .card__content {
  background-size: contain;
  background-repeat: no-repeat;
  background-position: center center;
  height: 100%;
  width: 100%;
}
</style>