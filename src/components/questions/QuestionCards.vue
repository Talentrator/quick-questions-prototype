<template>
  <div
    class="position-absolute top-0 bottom-0 w-100 flash-screen left-0 bg-white"
  >
    <question-card v-for="question in questions" :active="true" :question="question" :key="question.id" />
  </div>
</template>

<script>
import QuestionCard from "./QuestionCard.vue";
export default {
  components: { QuestionCard },
  name: "QuestionCards",
  emits: ["close"],
  data() {
    return {
      questions: [
        {
          id: 1,
          text: "Which of the following is not a core data type in Python programming?",
          language: "python",
          options: [
            { A: "Lists" },
            { B: "Tuples" },
            { C: "Dictionary" },
            { D: "Class" },
          ],
          answer: "D",
          time: 20, //in seconds
          score: 5,
        },
      ],
    };
  },
  methods: {},
};
</script>
<style lang="scss">
.flash-screen {
  z-index: 1000;
}
</style>