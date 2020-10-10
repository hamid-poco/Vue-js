<template>
  <div>
    <b-jumbotron>
      <template>{{ cQuestion.question }}</template>

      <hr class="my-4" />

      <b-list-group>
        <b-list-group-item
          v-for="(answer, i) in shuffledAnswers"
          :key="i"
          @click="changeIndex(i)"
          :class="correctFunction(i)"
        >
          {{ answer }}
        </b-list-group-item>
      </b-list-group>

      <b-button
        variant="primary"
        @click="submitAnswer"
        :disabled="Cindex == null || answered"
        href="#"
        >Submit</b-button
      >
      <b-button variant="success" href="#" @click="next">Next </b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";
export default {
  props: {
    cQuestion: Object,
    next: Function,
    increment: Function,
  },

  data() {
    return {
      Cindex: null,
      shuffledAnswers: [],
      correctAnswer: null,
      answered: false,
    };
  },

  computed: {
    answers() {
      let answers = [...this.cQuestion.incorrect_answers];
      answers.push(this.cQuestion.correct_answer);
      return answers;
    },
  },

  watch: {
    cQuestion: {
      immediate: true,
      handler() {
        (this.Cindex = null),
          (this.answered = null),
          (this.correctAnswer = null);
        this.shuffleAnswers();
      },
    },
  },
  methods: {
    changeIndex(i) {
      this.Cindex = i;
    },
    shuffleAnswers() {
      let answers = [
        ...this.cQuestion.incorrect_answers,
        this.cQuestion.correct_answer,
      ];
      this.shuffledAnswers = _.shuffle(answers);
      this.correctAnswer = this.shuffledAnswers.indexOf(
        this.cQuestion.correct_answer
      );
    },
    submitAnswer() {
      let is_correct = false;
      if (this.Cindex === this.correctAnswer) {
        is_correct = true;
      }
      this.answered = true;
      this.increment(is_correct);
    },
    correctFunction(index) {
      let answerClass = "";
      if (!this.answered && this.Cindex === index) {
        answerClass = "selected";
      } else if (this.answered && index === this.correctAnswer) {
        answerClass = "correct";
      } else if (
        this.answered &&
        index === this.Cindex &&
        index !== this.correctAnswer
      ) {
        answerClass = "incorrect";
      }
      return answerClass;
    },
  },
};
</script>

<style scoped>
.list-group-item:hover {
  background-color: #eee;
  cursor: pointer;
}

.btn {
  margin: 15px 3px;
}

.selected {
  background-color: lightblue;
}

.correct {
  background-color: lightgreen;
}

.incorrect {
  background-color: red;
}
</style>
