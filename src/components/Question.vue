<template>
  <div>
    <h3 class="my-5" v-html="currentQuestion.question"></h3>
    <p>Select one correct answer</p>
    <hr class="my-4" />
    <div class="my-4">
      <div v-for="answer in answers" :key="answer">
        <div
          :class="[
            answer === correctAns ? 'correct-ans' : 'answer',
            answer === incorrectAns ? 'incorrect-ans' : 'answer',
          ]"
          @click.prevent="selectAns(answer)"
          v-html="answer"
        ></div>
      </div>
    </div>
    <div class="d-flex justify-content-between">
      <b-button v-show="!(index === 0)" variant="primary" @click="previous"
        >Previous</b-button
      >
      <b-button
        variant="success"
        v-if="!(index === numOfQuestions - 1)"
        @click="next"
        >Next</b-button
      >
      <b-button variant="info" v-else @click="submitAns">Submit</b-button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Question",
  props: {
    currentQuestion: Object,
    next: Function,
    previous: Function,
    index: Number,
    numOfQuestions: Number,
    submitAns: Function,
  },
  data() {
    return {
      selectedAns: "",
      correctAns: "",
      incorrectAns: "",
    };
  },
  computed: {
    answers() {
      const shuffledAnswers = this.shuffle([
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer,
      ]);
      return shuffledAnswers;
    },
  },
  methods: {
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
    shuffle(array) {
      let currentIndex = array.length,
        randomIndex;

      // While there remain elements to shuffle...
      while (0 !== currentIndex) {
        // Pick a remaining element...
        randomIndex = Math.floor(Math.random() * currentIndex);
        currentIndex--;

        // And swap it with the current element.
        [array[currentIndex], array[randomIndex]] = [
          array[randomIndex],
          array[currentIndex],
        ];
      }

      return array;
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