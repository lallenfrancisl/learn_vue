<template>
  <div>
    <b-jumbotron>
      <template v-slot:lead>
        <div v-html="currentQuestion.question"></div>
      </template>
      <hr class="my-4">
      <b-list-group>
        <b-list-group-item v-html="answer" v-for="(answer, index) in shuffledAnswers" :key="index" @click="selectAnswer(index)" :class="{
            selected: index === selectedIndex && !answered,
            correct: index === correctIndex && answered,
            incorrect: (index !== correctIndex) && (selectedIndex === index) && answered
          }">
        </b-list-group-item>
      </b-list-group>
      <b-button @click="submitAnswer" variant="success" href="#" class="mr-2 mt-3" :disabled="(selectedIndex === null) || answered">
        Submit
      </b-button>
      <b-button @click="next" variant="primary" href="#" class="mr-2 mt-3" :disabled="totalCount >= 10 || !answered">
        Next
      </b-button>
      <p v-show="totalCount >= 10" class="text-center mt-3 mb-0">
        Quiz Complete!!! Hurray
      </p>
    </b-jumbotron>
  </div>
</template>
<script>
import _ from 'lodash';

export default {
  props: {
    currentQuestion: Object,
    totalCount: Number,
    next: Function,
    previous: Function,
    increment: Function
  },
  data() {
    return {
      selectedIndex: null,
      correctIndex: null,
      answered: true,
      shuffledAnswers: []
    }
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.answered = false;
        this.shuffleAnswers();
      }
    }
    // this is another way to do it
    // currentQuestion() {
    //   this.selectedIndex = null;
    //   this.shuffleAnswers();
    // }
  },
  methods: {
    selectAnswer(index) {
      console.log(index);
      this.selectedIndex = index;
    },
    shuffleAnswers() {
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer];

      this.shuffledAnswers = _.shuffle(answers);
      this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer);
    },
    submitAnswer() {
      let isCorrect = false;

      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }

      this.answered = true;

      this.increment(isCorrect);
    },
    answerClass(index) {
      let answerClass = '';

      if (index === this.selectedIndex && !this.answered) {
        answerClass = 'selected';
      } else if (index === this.correctIndex && this.answered) {
        answerClass = 'correct';
      } else if ((index !== this.correctIndex) &&
        (this.selectedIndex === index) &&
        this.answered
      ) {
        answerClass = 'incorrect';
      } else {
        answerClass = '';
      }

      return answerClass;
    }
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);

      return answers;
    }
  }
}
</script>
<style scoped>
.list-group-item:hover {
  background: whitesmoke;
  cursor: pointer;
}

.selected {
  background: lightblue;
}

.correct {
  background: lightgreen;
}

.incorrect {
  background: red;
}
</style>
