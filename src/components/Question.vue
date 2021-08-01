<template>
  <div>
    <h3 class="my-5" v-html="currentQuestion.question"></h3>
    <h5 style="color: #3e3e3e">
      Scores {{ numOfCorrect }} out of {{ numOfTotal }}
    </h5>
    <p>Select one correct answer</p>
    <hr class="my-4" />
    <div class="my-4">
      <div v-for="answer in answers" :key="answer">
        <div
          :class="answerClass(answer)"
          @click.prevent="selectAns(answer)"
          v-html="answer"
        ></div>
      </div>
    </div>
    <div class="d-flex justify-content-between">
      <b-button
        variant="info"
        :disabled="selectedAns === '' || answered"
        @click="submitAns"
        >Submit</b-button
      >
      <b-button
        variant="success"
        :disabled="index === numOfQuestions - 1"
        @click="
          () => {
            next();
            //reset
            submitAns(selectedAns);
            answered = false;
            selectedAns = '';
            correctAns = '';
            incorrectAns = '';
          }
        "
        >Next</b-button
      >
    </div>
  </div>
</template>

<script>
import { shuffle } from "../utils";

export default {
  name: "Question",
  props: {
    currentQuestion: Object,
    next: Function,
    previous: Function,
    index: Number,
    numOfQuestions: Number,
    // submitAns: Function,
  },
  data() {
    return {
      selectedAns: "",
      correctAns: "",
      incorrectAns: "",
      answered: false,
      numOfTotal: 0,
      numOfCorrect: 0,
    };
  },
  computed: {
    answers() {
      const shuffledAnswers = shuffle([
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer,
      ]);
      return shuffledAnswers;
    },
  },
  methods: {
    answerClass(answer) {
      let className = "";

      if (!this.answered && this.selectedAns === answer) {
        className = "answer selected-ans";
      } else if (this.answered && answer === this.correctAns) {
        className = "answer correct-ans";
      } else if (
        this.answered &&
        this.selectedAns === answer &&
        answer === this.incorrectAns
      ) {
        className = "answer incorrect-ans";
      } else {
        className = "answer";
      }

      return className;
    },
    selectAns(ans) {
      this.selectedAns = ans;
      if (this.selectedAns === this.currentQuestion.correct_answer) {
        this.correctAns = ans;
        this.incorrectAns = "";
      } else if (
        this.currentQuestion.incorrect_answers.includes(this.selectedAns)
      ) {
        this.incorrectAns = ans;
        this.correctAns = "";
      }
    },
    submitAns() {
      if (this.selectedAns === this.currentQuestion.correct_answer) {
        this.numOfCorrect++;
      }
      this.numOfTotal++;
      this.answered = true;
    },
  },
};
</script>

<style scoped>
.answer {
  padding: 5px;
  border: solid blue 1px;
  cursor: pointer;
  transition: all 0.16s linear;
}
.answer:hover {
  transform: scale(1.008);
  background-color: #f0f0f0;
}
.selected-ans {
  padding: 5px;
  border: solid blue 1px;
  background-color: #adadff;
}
.correct-ans {
  background-color: #13cf2c;
  color: #ffffff;
}
.incorrect-ans {
  background-color: #cf1313;
  color: #ffffff;
}
</style>