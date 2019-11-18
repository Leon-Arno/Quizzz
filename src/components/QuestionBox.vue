<template>
  <div>
    <b-jumbotron>
      <template slot="lead">{{currentQuestion.question}}</template>
      <hr class="my-4" />

      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in answers"
          :key="index"
          @click="selectedOption(index)"
          :class="answerClass(index)"
        >{{answer}}</b-list-group-item>
      </b-list-group>

      <b-button
        variant="primary"
        @click="submitAnswer"
        :disabled="selectedIndex === null || answered"
      >Submit</b-button>
      <b-button @click="next" variant="success" href="#">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";

export default {
  props: {
    currentQuestion: Object,
    next: Function,
    incrementCounter: Function
  },
  data() {
    return {
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      answered: false
    };
  },
  watch: {
    // currentQuestion:{
    //   immediate: true,
    //   handler(){
    //       this.selectedIndex = null;
    //   this.shuffleAnswers();
    //   }
    // }
    currentQuestion() {
      this.selectedIndex = null;
      this.shuffleAnswers();
      this.answered = false;
    }
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    }
  },
  methods: {
    selectedOption(index) {
      this.selectedIndex = index;
    },
    shuffleAnswers() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer
      ];
      this.shuffledAnswers = _.shuffle(answers);
      this.correctIndex = this.shuffledAnswers.indexOf(
        this.currentQuestion.correct_answer
      );
    },
    submitAnswer() {
      let isCorrectAnswer = false;

      this.selectedIndex === this.correctIndex
        ? (isCorrectAnswer = true)
        : false;
      this.answered = true;

      this.incrementCounter(isCorrectAnswer);
    },
    answerClass(index) {
      let answerClass = "";
      !this.answered && this.selectedIndex === index
        ? (answerClass = "selected")
        : this.answered && this.correctIndex === index
        ? (answerClass = "correct")
        : this.answered && this.selectedIndex === index
        ? (answerClass = "incorrect")
        : "";
      return answerClass;
    }
  },
  mounted() {
    this.shuffleAnswers();
  }
};
</script>

<style scoped>
.list-group {
  margin: 0 0 20px 0;
}
.list-group:hover {
  background: efefef;
  cursor: pointer;
}
.selected {
  background: rgba(6, 115, 231, 0.765);
}
.correct {
  background: rgb(53, 221, 53);
}
.incorrect {
  background: red;
}
.btn {
  margin: 10px;
}
</style>

