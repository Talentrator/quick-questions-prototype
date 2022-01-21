<template>
  <div
    class="question-card h-100 p-3"
    @touchstart="handleTouchStart"
  >
    <b-row class="justify-content-center align-items-center h-100">
      <div class="text-center px-3">
        <p class="question fs-5">
          {{ question.text }}
        </p>
        <b-row class="px-3 mt-5">
          <b-col cols="6" class="text-md-center text-start">
            <span>
              {{ formatTime(question.time) }}
            </span>
          </b-col>
          <b-col cols="6" class="text-md-center text-end">
            <span class=""> {{ question.score }} points </span>
          </b-col>
        </b-row>
      </div>
    </b-row>
  </div>
</template>

<script>
export default {
  name: "QuestionCard",
  props: {
    question: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      xDown: null,
      yDown: null,
    };
  },
  methods: {
    formatTime(seconds) {
      const minutes = parseInt(seconds / 60);
      seconds = seconds % 60;
      const minutesFormatted = minutes ? `${minutes} min ` : "";
      const secondssFormatted = seconds ? `${seconds} sec` : "";
      return `${minutesFormatted}${secondssFormatted}`;
    },
    handleTouchStart(evt) {
      const firstTouch = evt.touches[0];
      this.xDown = firstTouch.clientX;
      this.yDown = firstTouch.clientY;

      addEventListener("touchmove", this.handleTouchMove, false);
    },
    handleTouchMove(evt) {
      if (!this.xDown || !this.yDown) {
        return;
      }

      var xUp = evt.touches[0].clientX;
      var yUp = evt.touches[0].clientY;

      var xDiff = this.xDown - xUp;
      var yDiff = this.yDown - yUp;

      if (Math.abs(xDiff) > Math.abs(yDiff)) {
        /*most significant*/
        if (xDiff < 0) {
          /* right swipe */
          console.log("right");
        } else {
          /* left swipe */
          console.log("left");
        }
      } else {
        if (yDiff < 0) {
          /* down swipe */
          console.log("down");
        } else {
          /* up swipe */
          console.log("up");
        }
      }
      /* reset values */
      this.xDown = null;
      this.yDown = null;
    },
  },
};
</script>