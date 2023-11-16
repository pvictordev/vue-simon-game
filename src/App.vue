<template>
  <div class="simon-game">
    <div class="buttons">
      <div
        v-for="(button, index) in buttons"
        :key="index"
        class="button"
        :class="`button-${index}`"
        @click="handleButtonClick(index)"
      ></div>
    </div>
    <button @click="startGame">Start Game</button>
    <div v-if="gameOver" class="game-over">Game Over!</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      buttons: [false, false, false, false],
      sequence: [],
      playerSequence: [],
      level: 1,
      gameOver: false,
      difficulty: {
        easy: 1500,
        normal: 1000,
        hard: 400
      }
    }
  },
  methods: {
    startGame() {
      this.gameOver = false
      this.level = 1
      this.nextTurn()
    },
    nextTurn() {
      this.playerSequence = []
      this.sequence.push(Math.floor(Math.random() * 4))
      this.playSequence(this.sequence)
    },
    playSequence(sequence) {
      let i = 0
      const interval = setInterval(() => {
        this.activateButton(sequence[i])
        i++
        if (i >= sequence.length) {
          clearInterval(interval)
          setTimeout(() => {
            this.buttons = [false, false, false, false]
          }, 300)
        }
      }, this.difficulty.easy)
    },
    activateButton(index) {
      this.buttons = [false, false, false, false]
      this.buttons[index] = true
      this.playSound(index)
    },
    playSound(index) {
      console.log(`Playing sound for button ${index}`)
    },
    handleButtonClick(index) {
      if (!this.gameOver) {
        this.activateButton(index)
        this.playerSequence.push(index)
        const seqLength = this.sequence.length
        if (this.playerSequence[seqLength - 1] !== this.sequence[seqLength - 1]) {
          this.gameOver = true
        } else if (seqLength === this.playerSequence.length) {
          setTimeout(() => {
            this.nextTurn()
          }, 1000)
        }
      }
    }
  }
}
</script>

<style src="./assets/app.css">

</style>
