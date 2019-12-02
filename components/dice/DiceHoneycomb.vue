<template>
  <div class="dice-honeycomb">
    <transition
      name="odometr"
      @before-enter="beforeEnter"
      @after-enter="afterEnter"
    >
      <ul
        ref="digits"
        :key="gameCounter"
        class="dice-honeycomb-digits"
        :class="classObj"
      >
        <li
          v-for="(item, index) in outArray"
          :key="'d' + index"
          class="dice-honeycomb-digit"
        >
          {{ item }}
        </li>
      </ul>
    </transition>
  </div>
</template>

<script>
export default {
  name: 'DiceHoneycomb',
  props: {
    digit: {
      type: Number,
      required: true
    },
    delay: {
      type: Number,
      required: true
    },
    gameCounter: {
      type: Number,
      required: true
    }
  },
  data () {
    return {
      digitsArray: [this.digit],
      animated: false
    }
  },
  computed: {
    outArray () {
      return this.digitsArray
    },
    classObj () {
      const styleName = 'delay-' + this.delay
      return {
        [styleName]: true,
        'animated': this.animated
      }
    }
  },
  methods: {
    beforeEnter () {
      // Создаём массив цифр
      const digitsArray = []
      const oldDigit = this.digitsArray[this.digitsArray.length - 1]
      for (let i = oldDigit; i >= 0; i--) {
        digitsArray.push(i)
      }
      for (let i = 9; i >= 0; i--) {
        digitsArray.push(i)
      }
      for (let i = 9; i >= parseInt(this.digit); i--) {
        digitsArray.push(i)
      }
      this.digitsArray = digitsArray
    },
    afterEnter () {
      this.animated = true
      this.digitsArray = this.digitsArray.slice(-1)
      setTimeout(() => {
        this.animated = false
      }, 200)
    }
  }
}
</script>

<style>
.dice-honeycomb-wrap::before,
.dice-honeycomb-wrap::after {
  content: '';
  display: block;
  position: absolute;
  left: 0;
  right: 0;
  z-index: 2;
}

.dice-honeycomb-wrap::before {
  top: 0;
  height: 3vw;
}

.dice-honeycomb-wrap::after {
  bottom: 0;
  height: 3.8vw;
}

.dice-honeycomb {
  height: 105px;
  margin-top: 15px;
  overflow: hidden;
}

  @media all and (max-width: 767px) {
    .dice-honeycomb {
      width: 100%;
      height: calc(100vw / 4 - 12%);
      margin-top: 12%;
      position: relative;
      z-index: 1;
    }
  }

.dice-honeycomb-digit {
  position: relative;
  font-size: 65px;
  font-weight: bold;
  color: #fff;
  line-height: 100px;
  text-align: center;
}

  @media all and (max-width: 767px) {
    .dice-honeycomb-digit {
      font-size: calc(100vw / 9.5);
      line-height: calc(100vw / 4 - 4.5vw);
      padding-top: 0px;
    }
  }

@media all and (max-width: 767px) {
  .dice-honeycomb-digits {
    margin-top: 0px;
  }
}

.odometr-enter-active {
  transition: transform 1s cubic-bezier(0.29, 0.01, .02, 0.99);
}
.odometr-enter-active.delay-100 { transition-delay: .1s }
.odometr-enter-active.delay-200 { transition-delay: .2s }
.odometr-enter-active.delay-300 { transition-delay: .3s }

.odometr-enter { transform: translateY(0); }
.odometr-enter-to { transform: translateY(calc(-100% + 93px)); }

.animated {
  animation: bounce .2s ease-in-out;
}

@keyframes bounce {
  0% {
      transform: translateY(-7px);
  }
  20% {
      transform: translateY(-7px);
  }
  40% {
      transform: translateY(-6px);
  }
  60% {
      transform: translateY(-3px);
  }
  80% {
      transform: translateY(3px);
  }
  100% {
      transform: translateY(0);
  }
}

  @media all and (max-width: 767px) {
    .odometr-enter-to { transform: translateY(calc(-100% + 17.7vw)); }
    @keyframes bounce {
      0% {
        transform: translateY(-2.6vw);
      }
      20% {
        transform: translateY(-2.6vw);
      }
      40% {
        transform: translateY(-2.4vw);
      }
      60% {
        transform: translateY(-1.7vw);
      }
      80% {
        transform: translateY(1vw);
      }
      100% {
        transform: translateY(0);
      }
    }
  }
</style>
