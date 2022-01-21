<template>
  <div class="question-card h-100 p-3 position-relative">
      <b-row class="justify-content-center align-items-center h-100">
          <div class="">
              Results
          </div>
      </b-row>
  </div>
</template>

<script>
export default {
  name: "ResultsCard",
  props: {
    answers: {
      type: Array,
      required: true,
    },
    questions: {
      type: Array,
      required: true,
    },
  },
};
</script>
