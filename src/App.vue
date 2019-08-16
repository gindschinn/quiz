<template>
  <div id="app">
    <Header
      :numCorrect="numCorrect"
      :numTotal="numTotal"
    />

    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="4" offset="4" class="cat_select_container">
            <b-form-select v-model="selected_category" :select-size="2">
              <option v-for="category in categories" v-bind:value="category.id">
                {{category.name}}
              </option>
            </b-form-select>
              <h2> {{currentCategory}} </h2>
        </b-col>

        <b-col sm="6" offset="3">
          <QuestionBox
            v-if="selected_category!= 0 && questions.length && index < 10"
            :currentQuestion = "questions[index]"
            :next="next"
            :increment="increment"
            :index="index"
          />
        </b-col>
      </b-row>
    </b-container>

  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'

export default {
  name: 'app',
  components: {
    Header,
    QuestionBox,
  },
  data(){
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0,
      categories: [],
      selected_category: '',
      currentCategory: 'Choose a category'
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
  color: #2c3e50;
  margin-top: 60px;
}
#radio-group-2{
  display: flex;
}
.cat_select_container{
  margin: 0.5rem 0;
}
.custom-select{
  text-align: center;
  margin-bottom: 1rem;
}
</style>
