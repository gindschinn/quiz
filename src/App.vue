<template>
  <div id="app">
    <Header/>

    <b-container class="bv-example-row">
      <b-row>
        <b-col class="cat_select_container col-sm-4">
            <div class="current_cat_container">
              <h2 v-if="currentCategory != 0"> {{currentCategory}} </h2>
              <h2 v-else>Category</h2>
            </div>
            <b-form-select v-model="selected_category" :select-size="1">
              <option v-for="category in categories" v-bind:value="category.id">
                {{category.name}}
              </option>
            </b-form-select>
        </b-col>

        <b-col class="col-sm-8">
          <div class="counter-container">
            <p v-show="currentCategory.length != 0">Points: {{numCorrect}}/{{numTotal}}</p>
          </div>
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
      currentCategory: '',
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
  background-color:#263056;
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
.current_cat_container h2 {
  color: white;
  font-size: 28px;
  text-transform: uppercase;
}
.counter-container {
  height: 60px;
}


</style>
