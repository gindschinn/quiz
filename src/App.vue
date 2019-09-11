<template>
  <div id="app">
    <Header
      :numCorrect="numCorrect"
      :numTotal="numTotal"
    />

    <b-container class="bv-example-row">
      <b-row>
        <b-col class="cat_select_container col-sm-4">
            <div class="current_cat_container">
              <h2> {{currentCategory}} </h2>
            </div>
            <b-form-select v-model="selected_category" :select-size="2">
              <option v-for="category in categories" v-bind:value="category.id">
                {{category.name}}
              </option>
            </b-form-select>
        </b-col>

        <b-col class="col-sm-8">
          <QuestionBox
            v-if="selected_category!= 0 && questions.length && index < 10"
            :currentQuestion = "questions[index]"
            :next="next"
            :increment="increment"
            :index="index"
          />
          <Message
            v-if="questions.length == 0 || index == 10"
            :numCorrect="numCorrect"
            :numTotal="numTotal"
          />
        </b-col>
      </b-row>
    </b-container>

  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'
import Message from './components/Message.vue'

export default {
  name: 'app',
  components: {
    Header,
    QuestionBox,
    Message,
  },
  data(){
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0,
      categories: [],
      selected_category: '',
      currentCategory: ''
    }
  },
  methods: {
    next() {
      this.index++
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.numCorrect++
      }
      this.numTotal++
    }
  },
  created: function(){
    fetch('https://opentdb.com/api_category.php', {method: 'get'})
    .then((response) => {
      return response.json()
    })
    .then((jsonData) =>{
      this.categories = jsonData.trivia_categories
    })
  },
  watch: {
    selected_category: function(){
      this.index = 0,
      this.numCorrect = 0,
      this.numTotal = 0,
      fetch(`https://opentdb.com/api.php?amount=10&category=${this.selected_category}&type=multiple`, {method: 'get'
      })
      .then((response) => {
        return response.json()
      })
      .then((jsonData) =>{
        this.questions = jsonData.results
        this.currentCategory = this.questions[0].category
      })
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: black;
  margin-top: 60px;
  background-color: rgb(255, 234, 234);
  min-height: 100vh;
}
#radio-group-2{
  display: flex;
}
.row {
  width: 100%;
}

.bv-example-row {
  padding: 3.5rem 0 ;
  display: flex;
  align-items: center;
}

.current_cat_container {
  height: 85px;
  margin-bottom: 1rem;
}
.custom-select{
  text-align: center;
  margin-bottom: 1rem;
  min-height: 400px;
  min-width: 100%;
}
.custom-select:focus{
  border-color: grey!important;
  -webkit-box-shadow: 0 0 0 0.2rem rgba(255, 17, 0, 0.25);
  box-shadow: 0 0 0 0.2rem rgba(255, 17, 0, 0.25)!important;
}


</style>
