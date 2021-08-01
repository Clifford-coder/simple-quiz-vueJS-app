<template>
  <div id="app">
    <b-container>
      <b-row>
        <Header :index="index" :numOfQuestions="questions.length" />
        <Questions :next="next" :questions="questions" :index="index" />
      </b-row>
    </b-container>
  </div>
</template>

<script>
import { v4 as uuidv4 } from "uuid";
import Header from "./components/Header.vue";
import Questions from "./components/Questions.vue";

//TODO: Improve the user experience of the application

export default {
  name: "App",
  components: {
    Header,
    Questions,
  },
  data() {
    return {
      questions: [],
      index: 0,
    };
  },
  methods: {
    next() {
      if (this.index === this.questions.length - 1) return;
      this.index++;
    },
  },
  mounted: function () {
    fetch("https://opentdb.com/api.php?amount=10&type=multiple", {
      method: "GET",
    })
      .then((res) => {
        return res.json();
      })
      .then((jsonData) => {
        //add ids since the api does not have it
        this.questions = jsonData.results.map((question) => ({
          ...question,
          id: uuidv4(),
        }));
      });
  },
};
</script>

<style>
.apppp {
  color: aliceblue;
}
</style>
