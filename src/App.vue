<template>
  <GameResult 
    :correctAnswerCount="this.correctAnswerCount" 
    :wrongAnswerCount="this.wrongAnswerCount"
  />
  <template v-if="this.answers">
    <div class="questionBorad">
      <h1 v-html="this.question"></h1>
      <template v-for="answer in this.answers" :key="answer">
        <input 
          type="radio" 
          name="options" 
          :value="answer" 
          v-model="this.choosenAnswer" 
          :disabled="this.answerSubmitted"
          class="radioButton"
        />
        <label v-html="answer"></label><br />
      </template>
      <button @click="this.submitAnswer()" v-if="!this.answerSubmitted && this.question">Submit</button>
      <section v-if="this.answerSubmitted">
        <h4 v-if="this.choosenAnswer === this.correctAnswer" v-html="'&#9989; Congratulations, the answer ' + this.correctAnswer +  ' is correct.'"></h4>
        <h4 v-else v-html="'&#10060; I`am sorry you picked a wrong answer. The correct answer is ' + this.correctAnswer + '.'"></h4>
        <button @click="this.getQuizQuestion()">Next question</button>
      </section>
    </div>
  </template>
</template>

<script>
import GameResult from './components/GameResult.vue'
export default {
  name: 'App',
  components: {
    GameResult
  },
  data() {
    return {
      question: undefined,
      correctAnswer: undefined,
      incorrectAnswer: undefined,
      choosenAnswer: undefined,
      answerSubmitted: false,
      correctAnswerCount: 0,
      wrongAnswerCount: 0,
    }
  },
  methods: {
    getQuizQuestion: function() {
      this.question = undefined
      this.answerSubmitted = false
      this.choosenAnswer = undefined
      this.correctAnswer = undefined
      this.incorrectAnswer = undefined
      try {
      this.axios.get('https://opentdb.com/api.php?amount=1&category=9').then(response => {
        this.question = response.data.results[0].question;
        this.correctAnswer = response.data.results[0].correct_answer
        this.incorrectAnswer = response.data.results[0].incorrect_answers
      })
      } catch (e) {
        alert("something went wrong")
      }
    },
    submitAnswer() {
      if(!this.choosenAnswer) {
        alert("please select answer to submit")
      } else {
        this.answerSubmitted = true
        if(this.choosenAnswer === this.correctAnswer) {
          this.correctAnswerCount++
        } else {
          this.wrongAnswerCount++
        }
      }
      
    }
  },
  computed: {
    answers() {
      if(this.incorrectAnswer) {
        var answers = [...this.incorrectAnswer]
        answers.splice(Math.round(Math.random() * answers.length), 0 , this.correctAnswer)
        return answers;
      } else {
        return []
      }
    }
  },
  created() {
    this.getQuizQuestion();
  }
  
}
</script>

<style lang="scss">
body {
  background-color: aliceblue;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;

  button {
    padding: 0.75rem;
    background-color: rgb(47, 47, 172);
    color: #fff;
    margin-top: 1rem;
    border-radius: 0.75rem;
  }
  .radioButton {
    text-align: left;
    margin-top: 0.5rem;
    margin-right: 0.5rem;
  }
  .questionBorad {
    border-radius: 1rem;
    background-color: white;
    width: 80%;
    margin: auto;
    padding: 2rem;
  }
}
@media only screen and (max-width: 768px) {
  h1 {
    font-size: 16px;
  }
  h2 {
    font-size: 20px;
  }
}
</style>
