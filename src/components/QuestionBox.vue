<template>
  <div>
    <b-jumbotron>

      <template slot="lead">
        <div v-html="currentQuestion.question">
          {{currentQuestion.question}}
        </div>
      </template>

      <hr class="my-4">

      <b-list-group>
        <b-list-group-item
          v-html="answer"
          v-for= "(answer, index) in shuffledAnswers"
          :key="index"
          @click.prevent="selectAnswer(index)"
          :class="answerClass(index)"
        >
          {{answer}}
        </b-list-group-item>
      </b-list-group>

      <b-button
        variant="primary"
        @click="submitAnswer"
        :disabled="selectedIndex === null || answered"
      >
        Submit
      </b-button>
      <b-button @click="next" variant="success">
        Next
      </b-button>
      <div v-show="index>9">
        <p>Feddich</p>
      </div>
    </b-jumbotron>
  </div>
</template>

<script>
  import _ from 'lodash'

  export default{
    props: {
      currentQuestion: Object,
      next: Function,
      increment: Function,
      index: Number
    },
    data() {
      return {
        selectedIndex: null,
        correctIndex: null,
        shuffledAnswers: [],
        answered: false,
      }
    },
    // computed: {
    //   answers() {
    //     let answers = [...this.currentQuestion.incorrect_answers]
    //     answers.push(this.currentQuestion.correct_answer)
    //     return answers
    //   }
    // },
    watch: {
      currentQuestion: {
        immediate: true,
        handler(){
          this.selectedIndex = null
          this.answered = false
          this.shuffleAnswers()
        }
      }
    },
    methods: {
      selectAnswer(index){
        this.selectedIndex = index
      },
      submitAnswer(){
        let isCorrect = false

        if (this.selectedIndex === this.correctIndex) {
          isCorrect = true
        }
        this.answered = true
        this.increment(isCorrect)
      },
      shuffleAnswers(){
        let answers = [...this.currentQuestion.incorrect_answers,this.currentQuestion.correct_answer]
        this.shuffledAnswers = _.shuffle(answers)
        this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
      },
      answerClass(index){
        let answerClass = ''

        if (!this.answered && this.selectedIndex === index){
          answerClass = 'selected'
        } else if (this.answered && this.correctIndex === index){
          answerClass = 'correct'
        } else if (this.answered && this.selectedIndex === index && this.correctIndex !== index){
          answerClass = 'incorrect'
        }
        return answerClass
      }
    }
  }
</script>


<style scoped>
  .jumbotron {
      padding: 2rem 2rem;
    }
  .list-group{
    margin-bottom: 1rem;
  }
  .list-group-item:hover{
    background: #eee;
    cursor: pointer
  }
  .btn{
    margin: 0 0.5rem;
  }
  .btn:disabled{
    background: lightgrey;
    border: 0;
  }
  .selected{
    background: lightblue;
  }
  .correct{
    background: lightgreen;
  }
  .incorrect{
    background: red;
  }
</style>
