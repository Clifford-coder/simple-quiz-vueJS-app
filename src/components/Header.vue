<template>
  <div>
    <b-nav tabs>
      <b-nav-item disabled><b>LC Quiz App</b></b-nav-item>
      <b-nav-item disabled
        >Question :
        <b>{{ index + 1 }} out of {{ numOfQuestions }}</b></b-nav-item
      >
    </b-nav>
  </div>
</template>

<script>
export default {
  name: "Header",
  props: {
    index: Number,
    numOfQuestions: Number,
  },
};
</script>