<template>
  <div id="app">
    <Header 
      :numberCorrect="numberCorrect"
      :numberTotal="numberTotal"
    />
    <br>
    <b-container class="bv-example-row">
      <b-row >
       <b-col sm="6" offset="3">
          <Question 
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :nextQuestion="nextQuestion"
            :increment="increment"
          />
       </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import axios from 'axios'
import Header from './components/Header'
import Question from './components/Question'

export default {
  name: 'App',
  components: {
    Header,
    Question
  },
  data(){
    return {
      questions : [],
      index : 0,
      numberCorrect : 0,
      numberTotal : 0
    }
  },
  created(){
      axios.get('https://opentdb.com/api.php?amount=10&category=9&difficulty=medium&type=multiple')
          .then(res => this.questions = res.data.results)
          .catch(error => console.log(error))
  },
  methods : {
    nextQuestion(){
      this.index ++
    },
    increment(isCorrect){
      if(isCorrect){
        this.numberCorrect++
      }
      this.numberTotal++
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
