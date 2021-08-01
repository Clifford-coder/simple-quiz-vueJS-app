<template>
  <div class="d-flex justify-content-center">
    <b-jumbotron>
      <Question
        v-if="questions.length"
        :next="next"
        :previous="previous"
        :submitAns="submitAns"
        :index="index"
        :numOfQuestions="questions.length"
        :currentQuestion="questions[index]"
      />
    </b-jumbotron>
  </div>
</template>

<script>
import Question from "./Question.vue";

export default {
  name: "Questions",
  props: {
    questions: Array,
    index: Number,
    next: Function,
    previous: Function,
    submitAns: Function,
  },
  components: {
    Question,
  },
  methods: {},
};
</script>