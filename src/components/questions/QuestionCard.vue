<template>
  <div class="question-card h-100 p-3">
      <b-row class="justify-content-center align-items-center h-100">
          <div class="text-center px-3">
              <p class="question fs-5">
                  {{question.text}}
              </p>
            <b-row class="px-3 mt-4">
                <b-col col="6">
                    <span class="">
                        {{formatTime(question.time)}}
                    </span>
                </b-col>
                <b-col col="6" class="text-center">
                    <span class="">
                        {{question.score}} points
                    </span>
                </b-col>
            </b-row>
          </div>
      </b-row>
  </div>
</template>

<script>
export default {
  name: "QuestionCard",
  data() {
    return {
      question: {
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
    };
  },
  methods: {
      formatTime(seconds){
          const minutes = parseInt(seconds / 60);
          seconds = seconds % 60;
          const minutesFormatted = minutes ? `${minutes} min ` : '';
          const secondssFormatted = seconds ? `${seconds} sec` : '';
          return `${minutesFormatted}${secondssFormatted}`;
      }
  },
};
</script>