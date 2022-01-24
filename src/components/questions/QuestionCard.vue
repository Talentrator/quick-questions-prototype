<template>
  <div
    :class="[
      'question-card h-100 p-3 position-relative',
      { 'd-none': !active },
    ]"
    @touchstart="handleTouchStart"
  >
    <div class="answer up start-0 text-center w-100">
      <p class="">List</p>
      <arrow-scroll-animation class="d-inline-block my-2" direction="up" />
      <p class="fw-bold">A</p>
    </div>
    <div class="answer down start-0 text-center w-100">
      <p class="fw-bold">C</p>
      <arrow-scroll-animation class="d-inline-block my-2" />
      <p class="">Dictionary</p>
    </div>
    <b-row class="justify-content-center align-items-center h-100">
      <div class="text-start text-md-center px-3 col-md-8">
        <p class="question fs-5">
          {{ question.text }}
        </p>
        <!-- time and score -->
        <b-row class="mx-0 mt-5">
          <b-col cols="6" class="text-md-center text-start">
            <span>
              {{ formatTime(remainingTime) }}
            </span>
          </b-col>
          <b-col cols="6" class="text-md-center text-end">
            <span class=""> {{ question.score }} points </span>
          </b-col>
        </b-row>

        <b-row class="text-start px-3 mt-4">
          <b-col cols="6" class="d-flex">
            <div class="text-start">
              <arrow-scroll-animation
                class="d-inline-block mb-1"
                direction="left"
              />
              <div class="">
                <p class="fw-bold">D</p>
                <p class="">Class</p>
              </div>
            </div>
          </b-col>
          <b-col cols="6" class="text-end">
            <div class="text-end position-relative">
              <arrow-scroll-animation
                class="d-inline-block mb-1"
                direction="right"
              />
              <div class="">
                <p class="fw-bold">B</p>
                <p class="">Tuples</p>
              </div>
            </div>
          </b-col>
        </b-row>
      </div>
    </b-row>
  </div>
</template>

<script>
import ArrowScrollAnimation from "./ArrowScrollAnimation.vue";
import Time from '@/mixins/TimeMixin.js';
export default {
  components: { ArrowScrollAnimation },
  name: "QuestionCard",
  emits: ["answered"],
  mixins: [Time],
  props: {
    question: {
      type: Object,
      required: true,
    },
    active: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      xDown: null,
      yDown: null,
      userAnswer: null,
      gestureAnswers: [
        {
          direction: "up",
          answer: "A",
        },
        {
          direction: "right",
          answer: "B",
        },
        {
          direction: "down",
          answer: "C",
        },
        {
          direction: "left",
          answer: "D",
        },
      ],
      remainingTime: this.question.time,
      questionTimer: null,
    };
  },
  watch: {
    active(newValue) {
      if (newValue) {
        this.startCounter();
      }
    },
  },
  computed: {
    canAnswer() {
      return this.remainingTime > 0;
    },
  },
  methods: {
    startCounter() {
      this.questionTimer = setInterval(() => {
        if (this.remainingTime <= 0) {
          this.stopCounter();
          return;
        }
        this.remainingTime -= 1;
      }, 1000);
    },
    stopCounter() {
      if (this.questionTimer) clearInterval(this.questionTimer);
    },
    findGestureAnswer(dir) {
      let answ = this.gestureAnswers.find(({ direction }) => direction == dir);
      return answ ? answ.answer : answ;
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

      let xUp = evt.touches[0].clientX;
      let yUp = evt.touches[0].clientY;

      let xDiff = this.xDown - xUp;
      let yDiff = this.yDown - yUp;

      let direction = "";

      if (Math.abs(xDiff) > Math.abs(yDiff)) {
        /*most significant*/
        direction = xDiff < 0 ? "right" : "left";
      } else {
        direction = yDiff < 0 ? "down" : "up";
      }
      //
      this.userAnswer = this.findGestureAnswer(direction);
      this.answer(this.userAnswer);
      /* reset values */
      this.xDown = null;
      this.yDown = null;
    },
    answer(answ) {
      this.$emit("answered", {
        answer: answ,
        questionId: this.question.id,
        duration: this.question.time - this.remainingTime,
      });
      this.stopCounter();
    },
  },
  created() {
    if (this.active) this.startCounter();
  },
  beforeDestroy() {
    console.log("distroyed");
    this.$el.removeEventListener("touchstart", () => {
      this.xDown = null;
      this.yDown = null;
    });
    this.$el.removeEventListener("touchmove", () => {
      this.xDown = null;
      this.yDown = null;
    });
  },
};
</script>

<style scoped lang="scss">
.question-card {
  .answer {
    position: absolute;
    &.up {
      top: 5%;
    }
    &.down {
      bottom: 5%;
    }
  }
}
</style>