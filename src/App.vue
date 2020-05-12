<template>
  <div id="app">
    <Header
      :numCorrect="numCorrect"
      :numTotal="numTotal"
     />
    <b-container>
      <b-row>
        <b-col sm="8" offset="2">
          <QuestionBox 
            v-if="questions.length"
            :currentQuestion="questions[index]" 
            :next="next"
            :increment="increment"
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
  name: 'App',
  components: {
    Header,
    QuestionBox
  },
  data: function() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0
    }
  },
  mounted: function() {
    fetch('https://opentdb.com/api.php?amount=10&category=27&type=multiple', {
      method: 'get'
    })
      .then(response => response.json())
      .then(jsonData => this.questions = jsonData.results)
  },
  methods: {
    next() {
      this.index++
    },
    increment(isCorrect) {
      if(isCorrect) {
        this.numCorrect++
      } 
      this.numTotal++
    }
  }
}
</script>
