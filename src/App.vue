<template>
  <div class="simon-game">
    <div class="header">
      <p class="level">Уровень: {{ level }}</p>
      <h1 class="title">Simon The Game</h1>
      <div class="difficulty">
        <p>Сложность:</p>
        <select class="difficulty-buttons">
          <option class="easy">Легко</option>
          <option class="normal">Нормально</option>
          <option class="hard">Сложно</option>
        </select>
      </div>
    </div>

    <div class="buttons">
      <div
        v-for="(button, index) in buttons"
        :key="index"
        class="button"
        :class="`button-${index}`"
        @click="handleButtonClick(index)"
      ></div>
    </div>

    <button class="start-game" @click="startGame">Начать игру</button>
    <div v-if="gameOver" class="game-over">Провал!</div>
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

      const buttonElements = document.querySelectorAll('.button')
      buttonElements[index].classList.add('active')

      setTimeout(() => {
        buttonElements[index].classList.remove('active')
      }, this.difficulty.easy - 300)
    },
    playSound(index) {
      console.log(`Playing sound for button ${index}`)
      let audioSrc = ''

      switch (index) {
        case 0:
          audioSrc = 'https://s3.amazonaws.com/freecodecamp/simonSound2.mp3'
          break
        case 1:
          audioSrc = 'https://s3.amazonaws.com/freecodecamp/simonSound1.mp3'
          break
        case 2:
          audioSrc = 'https://s3.amazonaws.com/freecodecamp/simonSound4.mp3'
          break
        case 3:
          audioSrc = 'https://s3.amazonaws.com/freecodecamp/simonSound3.mp3'
          break

        default:
          audioSrc = 'http://soundbible.com/mp3/Kids%20Cheering-SoundBible.com-681813822.mp3'
          break
      }

      const audio = new Audio(audioSrc)

      audio.play()
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

<style src="./assets/app.css"></style>
