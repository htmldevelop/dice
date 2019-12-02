<template>
  <div class="content-wrap">
    <div class="dice-game-wrapper">
      <DiceGame />
    </div>
  </div>
</template>

<script>
import DiceGame from '~/components/dice/DiceGame.vue'

export default {
  head: {
    title: 'Dice'
  },
  components: {
    DiceGame
  }
}
</script>
<style>
.page {
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  white-space: nowrap;
  text-align: center;
  font-size: 0;
  background-color: #381960;
}

.page:before {
  height: 100%;
  display: inline-block;
  vertical-align: middle;
  content: '';
}
</style>
