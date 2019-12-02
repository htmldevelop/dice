<template>
  <div class="dice-game">
    <div class="dice-game-top">
      <!-- Score -->
      <div class="dice-score-wrapper">
        <DiceScore
          :score="score"
          :result-type="resultType"
        />
      </div>
    </div>
    <div class="dice-game-center">
      <!-- Result block -->
      <div class="dice-result-block">
        <!-- User Value -->
        <div class="dice-user-value-wrapper">
          <DiceUserValue
            :user-value="userValue"
            :reversed="reversed"
          />
        </div>
        <!-- Game result -->
        <div class="dice-result-wrapper">
          <DiceResult
            :game-value="gameValue"
            :game-counter="gameCounter"
            :result-type="resultType"
          />
        </div>
        <!-- History -->
        <div class="dice-history-wrapper">
          <DiceHistory
            :history="history"
            :game-counter="gameCounter"
          />
          <div class="dice-reverse-wrapper">
            <DiceReverse
              @reverse="reverse"
            />
          </div>
        </div>
        <div class="clearfix" />
      </div>
    </div>
    <div class="dice-game-bottom">
      <!-- Slider -->
      <div class="dice-slider-wrapper">
        <DiceSlider
          :user-value="userValue"
          :game-value="gameValue"
          :reversed="reversed"
          :game-counter="gameCounter"
          @changeUserValue="changeUserValue"
        />
        <div class="dice-reverse-wrapper">
          <DiceReverse
            @reverse="reverse"
          />
        </div>
      </div>
      <!-- Button -->
      <div class="dice-play-button-wrapper">
        <DicePlayButton
          @dicePlay="dicePlay"
        />
      </div>
    </div>
  </div>
</template>

<script>
import DiceScore from '~/components/dice/DiceScore.vue'
import DiceUserValue from '~/components/dice/DiceUserValue.vue'
import DiceResult from '~/components/dice/DiceResult.vue'
import DiceHistory from '~/components/dice/DiceHistory.vue'
import DiceSlider from '~/components/dice/DiceSlider.vue'
import DiceReverse from '~/components/dice/DiceReverse.vue'
import DicePlayButton from '~/components/dice/DicePlayButton.vue'

export default {
  name: 'DiceGame',
  components: {
    DiceScore,
    DiceUserValue,
    DiceResult,
    DiceHistory,
    DiceSlider,
    DiceReverse,
    DicePlayButton
  },
  data () {
    return {
      userValue: 50,
      gameValue: 0,
      reversed: false,
      score: 350.2,
      resultType: 0, // resultType: 0 - default, 1 - win, 2 - lose
      history: [],
      gameCounter: 0
    }
  },
  methods: {
    // Реверс: больше - меньше
    reverse () {
      this.reversed = !this.reversed
    },
    // Устанавливает новое занчение userValue по событию из слайдера
    changeUserValue (value) {
      this.userValue = value
    },
    // Начинает розыгрыш
    dicePlay () {
      // Сбрасываем в дефолт стили и классы элементов
      this.resultType = 0
      // Эмуляция получения ответа с сервера
      this.onServerResponse()
    },
    // Эмуляция получения ответа с сервера
    onServerResponse () {
      this.gameValue = this.generateResult()
      this.gameCounter++

      let tmp
      tmp = this.gameValue / 100
      if ((tmp <= this.userValue && this.reversed) || (tmp >= this.userValue && !this.reversed)) {
        tmp *= -1
      }
      this.history.push(tmp)

      setTimeout(() => {
        const snd = new Audio()
        if (tmp > 0) {
          this.resultType = 1
          snd.src = 'Win.mp3'
        } else {
          this.resultType = 2
          snd.src = 'Lose.mp3'
        }
        snd.volume = 0.4
        snd.autoplay = true
      }, 1200)
    },
    // Заглушка генерирующая рандомное число 0 - 9999
    generateResult () {
      return parseInt(Math.random() * 10000)
    }
  }
}
</script>

<style>
.dice-game-wrapper {
  display: inline-block;
  white-space: normal;
  vertical-align: middle;
  text-align: left;
}

  @media all and (max-width: 767px) {
    .dice-game-wrapper {
      width: 100%;
      height: 100%;
    }
  }

.dice-game {
  max-width: 813px;
  padding: 18px 35px 35px 35px;
  background-color: #291a43;
  border: 1px solid #000000;
  border-radius: 7px;
  font-family: "Source Sans Pro", sans-serif;
  font-weight: normal;
  font-stretch: normal;
  font-style: normal;
  letter-spacing: normal;
}

  @media all and (max-width: 767px) {
    .dice-game {
      display: flex;
      flex-direction: column;
      justify-content: center;
      width: 100%;
      height: 100%;
      padding: 15px 0 20px;
      border: 0;
      border-radius: 0;
      position: relative;
    }
  }

.dice-game-top,
.dice-game-bottom {
  flex: none;
}

.dice-game-center {
  position: relative;
}

.dice-user-value-wrapper {
  width: 121px;
  float: left;
}

  @media all and (max-width: 767px) {
    .dice-user-value-wrapper {
      width: 100%;
    }
  }

.clearfix {
  clear: both;
}
</style>
