<template>
  <div id="app">
    <Header :correct="correct" :total="total"/>
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset-lg="3">
          <QuestionBox
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :next="next"
            :incrementCounter="incrementCounter"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";

export default {
  name: "app",
  components: {
    Header,
    QuestionBox
  },
  data() {
    return {
      questions: [],
      index: 0,
      correct: 0,
      total: 0
    };
  },
  methods: {
    next() {
      this.index++;
    },
    incrementCounter(isCorrectAnswer) {
      if (isCorrectAnswer) {
        this.correct++;
      }
      this.total++;
    }
  },
  mounted: function() {
    fetch("https://opentdb.com/api.php?amount=10&category=9&type=multiple", {
      method: "get"
    })
      .then(response => response.json())
      .then(data => {
        this.questions = data.results;
      });
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
