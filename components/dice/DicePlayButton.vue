<template>
  <div
    class="dice-play-button"
    :class="{ 'clicked': clicked, 'disabled': disabled }"
    @click="play"
    @mousedown.left="clicked = true"
    @touchstart="clicked = true"
    @mouseup.left="clicked = false"
    @touchend="clicked = false"
    @mouseleave="clicked = false"
  >
    Играть
  </div>
</template>

<script>
export default {
  name: 'DicePlayButton',
  data () {
    return {
      disabled: false,
      clicked: false
    }
  },
  methods: {
    // Эмит сообщения о начале розыгрыша
    play () {
      this.disabled = true
      setTimeout(() => {
        this.disabled = false
      }, 1600)
      this.$emit('dicePlay')
    }
  }
}
</script>

<style>
.dice-play-button {
  width: 136px;
  height: 50px;
  padding: 13px 14px 16px 15px;
  border-radius: 5px;
  box-shadow: inset 0 -3px 0 0 rgba(0, 0, 0, 0.24);
  background-image: linear-gradient(to bottom, #ffed2a 5%, #ffdc2a 94%);
  margin: 0 auto;
  font-size: 18px;
  font-weight: bold;
  line-height: 21px;
  text-align: center;
  color: #3f2652;
}

@media all and (max-width: 767px) {
  .dice-play-button {
    width: 100%;
  }
  .dice-play-button-wrapper {
    padding: 0 10px;
  }
}

.dice-play-button:hover {
  cursor: pointer;
  box-shadow: inset 0 -3px 0 0 rgba(0, 0, 0, 0.24);
  background-image: linear-gradient(to bottom, #ffcb00 5%, #ffaf00 94%);
}

.dice-play-button.clicked {
  box-shadow: inset 0 3px 0 0 rgba(0, 0, 0, 0.24);
  background-color: #ffbb00;
}

.dice-play-button.disabled {
  pointer-events: none;
  opacity: .5;
}
</style>
