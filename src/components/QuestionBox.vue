<template>
  <div class="container">
    <b-jumbotron>

      <template slot="lead">
        <div class="question" v-html="currentQuestion.question">
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
      <b-button class="btn-nxt" @click="next" variant="success">
        Next
      </b-button>
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
  .container {
    display: flex;
    justify-content: center;
    align-items: baseline;
  }
  .jumbotron {
    padding: 2rem 2rem;
    margin: 0;
    background-color: lightcoral;
    border: 1px solid rgb(238, 110, 110);
    box-shadow: inset 10px 10px 5px -9px rgba(255,255,255,0.4), 10px 10px 5px -7px rgba(0,0,0,0.75);
    border-radius: 10px;
    width: 80%;
    height: 450px;
    }
  .lead {
    height: 100px;
  }
  .question {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100%;
    color: black;
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
  .btn-nxt {
    background-color:  #98DDDE;
    color: black;
    border: 1px solid rgb(111, 197, 199);
    box-shadow: 1px 1px 1px 1px rgba(0,0,0,0.3);
  }
  .btn-nxt:hover {
    background-color: rgb(73, 165, 167);
    border: none;
  }
  .btn:disabled{
    background: lightgrey;
    border: 0;
  }
  .selected{
    background: #98DDDE;
  }
  .correct{
    background: #a3c952;
  }
  .incorrect{
    background: rgb(255, 93, 115);
  }
</style>
