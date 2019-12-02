<template>
  <div class="dice-history">
    <transition name="list">
      <ul
        :key="gameCounter"
        class="dice-history-list"
      >
        <li
          v-for="item in formatHistory"
          :key="item"
          class="dice-history-item"
          :class="{ lose: item < 0 }"
        >
          {{ item | formatItem }}
        </li>
      </ul>
    </transition>
  </div>
</template>

<script>
export default {
  name: 'DiceHistory',
  filters: {
    formatItem (value) {
      return Math.abs(value).toFixed(2)
    }
  },
  props: {
    history: {
      type: Array,
      required: true
    },
    gameCounter: {
      type: Number,
      required: true
    }
  },
  computed: {
    formatHistory () {
      return this.history.slice(-6).reverse()
    }
  }
}
</script>

<style>
.dice-history {
  width: 53px;
  max-height: 133px;
  padding-top: 8px;
  overflow: hidden;
  position: relative;
}

  @media all and (max-width: 767px) {
    .dice-history {
      width: 100%;
      height: 22px;
      padding-top: 0;
      overflow: visible;
    }
  }

.dice-history-wrapper {
  width: 53px;
  float: right;
  margin-left: 63px;
}

  @media all and (max-width: 767px) {
    .dice-history-wrapper {
      width: calc(100% - 40px);
      float: none;
      padding-left: 10px;
      position: absolute;
      top: calc(100% - 37px);
      left: 0;
      overflow: hidden;
      margin-left: 0;
    }
  }

.dice-history-wrapper .dice-reverse-wrapper {
  display: none;
}

.dice-history-list {
  height: 165px;
}

@media all and (max-width: 767px) {
  .dice-history-list {
    display: flex;
    height: auto;
  }
}

.dice-history-item {
  display: block;
  height: 22px;
  padding: 3px;
  margin-bottom: 10px;
  text-align: center;
  color: #91ff4a;
  font-size: 12px;
  line-height: 16px;
  border-radius: 11.5px;
  background-color: rgba(255, 255, 255, 0.1);
}

@media all and (max-width: 767px) {
  .dice-history-item {
    flex: none;
    display: inline-block;
    width: calc(100vw / 7);
    margin-bottom: 0;
    margin-right: calc(100vw / 27);
  }
  .dice-history-item:last-child {
    margin-right: 0;
  }
}

.dice-history-item.lose {
  color: #ff6764;
}

.dice-history-item:nth-child(1) { opacity: 1; }
.dice-history-item:nth-child(2) { opacity: 0.7; }
.dice-history-item:nth-child(3) { opacity: 0.3; }
.dice-history-item:nth-child(4) { opacity: 0.1; }
.dice-history-item:nth-child(5) { opacity: 0; }

.list-enter .dice-history-item:nth-child(1) { opacity: 1; }
.list-enter .dice-history-item:nth-child(2) { opacity: 1; }
.list-enter .dice-history-item:nth-child(3) { opacity: 0.7; }
.list-enter .dice-history-item:nth-child(4) { opacity: 0.3; }
.list-enter .dice-history-item:nth-child(5) { opacity: 0.1; }

.list-enter-to .dice-history-item:nth-child(1) { opacity: 1; }
.list-enter-to .dice-history-item:nth-child(2) { opacity: 0.7; }
.list-enter-to .dice-history-item:nth-child(3) { opacity: 0.3; }
.list-enter-to .dice-history-item:nth-child(4) { opacity: 0.1; }
.list-enter-to .dice-history-item:nth-child(5) { opacity: 0; }

.list-enter-active .dice-history-item {
  transition: opacity .25s ease-out 1.3s;
}

/* Transition styles */
.list-enter-active { transition: transform .25s ease-out 1.3s; }
.list-enter { transform: translate3d(0, -32px, 0); }
.list-enter-to { transform: translate3d(0, 0, 0); }

@media all and (max-width: 767px) {
  .dice-history-item:nth-child(1) { opacity: 1; }
  .dice-history-item:nth-child(2) { opacity: 0.8; }
  .dice-history-item:nth-child(3) { opacity: 0.6; }
  .dice-history-item:nth-child(4) { opacity: 0.4; }
  .dice-history-item:nth-child(5) { opacity: 0.2;}
  .dice-history-item:nth-child(6) { opacity: 0; }

  .list-enter .dice-history-item:nth-child(1) { opacity: 1; }
  .list-enter .dice-history-item:nth-child(2) { opacity: 1; }
  .list-enter .dice-history-item:nth-child(3) { opacity: 0.8; }
  .list-enter .dice-history-item:nth-child(4) { opacity: 0.6; }
  .list-enter .dice-history-item:nth-child(5) { opacity: 0.4; }
  .list-enter .dice-history-item:nth-child(6) { opacity: 0.2; }

  .list-enter-to .dice-history-item:nth-child(1) { opacity: 1; }
  .list-enter-to .dice-history-item:nth-child(2) { opacity: 0.8; }
  .list-enter-to .dice-history-item:nth-child(3) { opacity: 0.6; }
  .list-enter-to .dice-history-item:nth-child(4) { opacity: 0.4; }
  .list-enter-to .dice-history-item:nth-child(5) { opacity: 0.2; }
  .list-enter-to .dice-history-item:nth-child(6) { opacity: 0.1; }

  /* Transition styles */
  .list-enter { transform: translateX(calc(100vw / 7 * -1 + 100vw / 27 * -1)) }
  .list-enter-to { transform: translateX(0); }
}
</style>
