<template>
  <div id="app">
    <Header
      :correctCount="correctCount"
      :totalCount="totalCount"
    />
    <b-container class="bv-example-row">
      <b-row class="justify-content-center">
        <b-col sm=6>
          <QuestionBox
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :next="next"
            :increment="increment"
            :totalCount="totalCount"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from './components/Header.vue';
import QuestionBox from './components/QuestionBox.vue';

export default {
  name: 'App',
  components: {
    Header,
    QuestionBox
  },
  data() {
    return {
      questions: [],
      index: 0,
      totalCount: 0,
      correctCount: 0
    }
  },
  methods: {
    next() {
      if (this.index < this.questions.length) {
        ++this.index;
      }
    },
    increment(isCorrect) {
      if (isCorrect) {
        ++this.correctCount;
      }

      ++this.totalCount;
    }
  },
  mounted: function() {
    fetch('https://opentdb.com/api.php?amount=10&type=multiple', {
      method: 'get'
    })
    .then((res) => {
      return res.json();
    })
    .then((jsonData) => {
      this.questions = jsonData.results;
    });
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
