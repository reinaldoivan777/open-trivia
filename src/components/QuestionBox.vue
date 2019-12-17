<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template v-slot:lead>{{ currentQuestion.question }}</template>

      <hr class="my-4" />

      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in shuffledAnswers"
          :key="index"
          @click="selectAnswer(index)"
          :class="answerClass(index)"
          :disabled="answered"
        >
          {{ answer }}
        </b-list-group-item>
      </b-list-group>

      <b-button
        variant="primary"
        :disabled="selectedIndex === null || answered"
        @click="submitAnswer"
        >Submit</b-button
      >
      <b-button variant="success" :disabled="!answered" @click="next"
        >Next</b-button
      >
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";
export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function
  },
  data() {
    return {
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      answered: false
    };
  },
  methods: {
    selectAnswer(index) {
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
      let isCorrect = false;

      if (this.selectedIndex === this.correctIndex) isCorrect = true;
      this.answered = true;

      this.increment(isCorrect);
    },
    answerClass(index) {
      let answerClass = "";

      if (!this.answered && this.selectedIndex === index)
        answerClass = "selected";
      else if (this.answered && this.correctIndex === index)
        answerClass = "correct";
      else if (
        this.answered &&
        this.selectedIndex === index &&
        this.correctIndex !== index
      )
        answerClass = "incorrect";

      return answerClass;
    }
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.correctIndex = null;
        this.answered = null;
        this.shuffleAnswers();
      }
    }
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    }
  }
};
</script>
<style scoped>
.list-group {
  margin-bottom: 15px;
  cursor: pointer;
}

.list-group-item:hover {
  background-color: #eee;
}

.btn {
  margin: 0 5px;
}

.selected {
  background-color: aqua;
}

.correct {
  background-color: green;
}

.incorrect {
  background-color: red;
}

.list-group-item.disabled.correct,
.list-group-item.disabled.incorrect {
  color: #ffffff;
}
</style>
