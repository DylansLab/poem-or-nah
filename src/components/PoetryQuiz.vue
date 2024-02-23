<template>
  <div class="poetry-quiz-box">
    <div class="score-total">
      {{ score }}
    </div>

    <div class="settings-box">
      <button @click="toggleAuthor">{{ authorToggleText }}</button>
      <button @click="openAbout">What is this?</button>
    </div>

    <div v-if="newRound" class="poems-box">
      <button
        @click="guessPoem(poem.real, poem.title, poem.author)"
        v-for="poem in poems"
        :key="poem.lines"
        class="poem"
      >
        <p v-for="line in poem.lines.slice(0, 4)" :key="line">
          {{ line }}
        </p>
        <br />
        <p v-for="line in poem.lines.slice(4, 7)" :key="line">
          {{ line }}
        </p>
        <br />
        <p v-if="authorToggle" class="poem-author">by {{ poem.author }}</p>
      </button>
    </div>

    <div v-if="showingResults == true" class="results-box">
      <div v-if="isCorrect" class="result correct">
        <h2>Correct! We told AI to write us this poem to sound like {{ author }}</h2>
      </div>
      <div v-else class="result incorrect">
        <h2>Incorrect! This is {{ title }} by {{ author }}.</h2>
      </div>

      <div class="action-box">
        <div class="score-box">
          <div class="score correct">
            <div class="label">{{ correct }}</div>
            <div class="value">Correct</div>
          </div>

          <div class="score total">
            <div class="label">{{ score }}</div>
            <div class="value">Total</div>
          </div>

          <div class="score incorrect">
            <div class="label">{{ incorrect }}</div>
            <div class="value">Incorrect</div>
          </div>
        </div>
        <div class="button-row">
          <button @click="newGame">New Game</button>
          <button @click="startRound">Next Round</button>
        </div>
      </div>
    </div>

    <AboutModal v-show="showingAbout" @close="closeAbout" />
  </div>
</template>

<script>
import AboutModal from './AboutModal.vue'
import AIPoems from '../assets/aipoems.json'

export default {
  name: 'PoetryQuiz',
  data() {
    return {
      poems: [],
      aipoems: [],
      realpoems: [],
      score: 0,
      correct: 0,
      incorrect: 0,
      title: '',
      author: '',
      authorToggle: true,
      authorToggleText: 'Hide Authors',
      isCorrect: true,
      newRound: false,
      showingResults: false,
      showingAbout: false
    }
  },
  props: {
    msg: String
  },
  methods: {
    shuffle(array) {
      let currentIndex = array.length,
        randomIndex

      // While there remain elements to shuffle.
      while (currentIndex > 0) {
        // Pick a remaining element.
        randomIndex = Math.floor(Math.random() * currentIndex)
        currentIndex--

        // And swap it with the current element.
        ;[array[currentIndex], array[randomIndex]] = [array[randomIndex], array[currentIndex]]
      }

      return array
    },
    guessPoem(poem, title, author) {
      if (poem == 'false') {
        this.isCorrect = true
        this.score++
        this.correct++
      } else {
        this.isCorrect = false
        this.score--
        this.incorrect++
      }

      this.title = title
      this.author = author
      this.newRound = false
      this.showingResults = true
    },
    toggleAuthor() {
      this.authorToggle = !this.authorToggle
      if (this.authorToggle == true) {
        this.authorToggleText = 'Hide Authors'
      } else {
        this.authorToggleText = 'Show Authors'
      }
    },
    newGame() {
      this.correct = 0
      this.incorrect = 0
      this.score = 0
      this.startRound()
    },
    openAbout() {
      this.showingAbout = true
    },
    closeAbout() {
      this.showingAbout = false
    },
    loadPoems() {
      fetch('https://poetrydb.org/random/100', {
        method: 'GET'
      })
        .then((response) => {
          response.json().then((res) => {
            res.forEach((poem) => {
              poem.real = 'true'
              if (poem.linecount > 16) {
                console.log(poem.linecount)
                poem.lines = poem.lines.splice(8, poem.lines.length)
              }
              this.realpoems.push(poem)
            })

            AIPoems.forEach((poem) => {
              poem.real = 'false'
              this.aipoems.push(poem)
            })

            this.shuffle(this.aipoems)
            this.shuffle(this.realpoems)
            this.startRound()
          })
        })
        .catch((err) => {
          console.error(err)
        })
    },
    startRound() {
      if (this.realpoems.length == 0 || this.aipoems.length == 0) {
        loadPoems()
      } else {
        this.poems = []
        this.showingResults = false
        this.newRound = true

        this.poems.push(this.realpoems.pop())
        this.poems.push(this.realpoems.pop())
        this.poems.push(this.aipoems.pop())

        this.shuffle(this.poems)
      }
    }
  },
  mounted: function () {
    this.loadPoems()
  },
  components: {
    AboutModal
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.poetry-quiz-box {
  background-color: lightsteelblue;
  padding: 20px;
  border: 5px solid lightslategray;
  border-radius: 15px;
  position: relative;
  margin: 0;
}

/* Score */

.poetry-quiz-box .score-total {
  position: absolute;
  top: 10px;
  left: 20px;
  font-size: 24px;
  font-weight: 500;
  color: aliceblue;
  border: 2px solid aliceblue;
  background-color: lightslategray;
  border-radius: 15px;
  padding: 1px;
  height: 40x;
  min-width: 40px;
  text-align: center;
}

/*Settings bar */

.settings-box {
  text-align: right;
}

.settings-box button {
  height: 100%;
  padding: 5px 10px;
  font-size: 12px;
  border: 5px solid lightslategray;
  border-radius: 15px;
  background-color: aliceblue;
  margin: auto 5px;
}

button:hover {
  background-color: lightsteelblue;
}

/*Poems Guessing Box*/
.poems-box {
  display: flex;
  padding: 15px;
  background-color: lightslategray;
  columns: 3;

  border: 5px solid lightslategray;
  border-radius: 15px;
  margin-top: 15px;
}

.poem {
  padding: 20px;
  margin: 20px;
  background-color: aliceblue;
  border: 5px solid lightslategray;
  border-radius: 15px;
}

.poem:hover {
  background-color: lightsteelblue;
}

.poem-author {
  font-weight: bold;
}

/*Scoring Results Box*/
.results-box {
  text-align: center;
  padding: 15px;
  margin: auto;
}

.results-box .result {
  color: black;
  padding: 10px 20px;
  margin: auto;
  border-radius: 15px;
  border: 5px solid lightslategray;
}

.results-box .result.correct {
  background-color: lightgreen;
}

.results-box .result.incorrect {
  background-color: lightsalmon;
}

.results-box .action-box {
  display: grid;
  justify-content: center;
}

.results-box .score-box {
  min-width: 500px;
  max-width: 1000px;

  display: flex;
  padding: 10px;
  border: 5px solid lightslategray;
  background-color: lightslategray;
  color: aliceblue;
  margin: 15px auto;
  border-radius: 15px;
}

.results-box .score-box.incorrect {
  background-color: lightcoral;
}

.results-box .score-box.correct {
  background-color: lightgreen;
}

.results-box .score-box .score {
  margin: auto;
  text-align: center;
}
.results-box .score-box .score .label,
.results-box .score-box .score .value {
  margin: auto;
  text-align: center;
  font-weight: 400;
  font-size: 20px;
}
.results-box .button-row {
  display: flex;
  padding: 10px;
}
.results-box button {
  height: 100%;
  padding: 10px 30px;
  font-size: 18px;
  border: 5px solid lightslategray;
  border-radius: 15px;
  background-color: aliceblue;
  margin: auto;
}

.results-box button:hover {
  background-color: lightsteelblue;
}

@media only screen and (max-width: 800px) {
  .poems-box {
    display: grid;
  }

  .poems-box .poem {
    max-width: fit-content;
    padding: 20px 60px;
    margin: 5px auto;
  }
}
</style>
