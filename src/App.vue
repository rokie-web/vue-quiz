<template>
  <div id="app">
    <Header />
    <b-container>
      <b-row>
        <b-col sm="8" offset="2">
          <QuestionBox :currentQuestion="questions[index]" />
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
      index: 0
    }
  },
  mounted: function() {
    fetch('https://opentdb.com/api.php?amount=10&category=27&type=multiple', {
      method: 'get'
    })
      .then(response => response.json())
      .then(jsonData => this.questions = jsonData.results)
  }
}
</script>
