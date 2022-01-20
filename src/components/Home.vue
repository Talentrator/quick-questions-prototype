<template>
  <div class="h-100">
    <b-container fluid class="h-100">
      <b-row class="h-100 justify-content-center align-items-center px-0">
        <div class="text-center">
          <b-button
            variant="primary"
            @click="showFlashScreen"
            class="px-4 py-2"
          >
            Start Questions
          </b-button>
        </div>
      </b-row>
    </b-container>
    <question-cards v-if="show" />
  </div>
</template>

<script>
import QuestionCards from "./questions/QuestionCards.vue";
export default {
  components: { QuestionCards },
  name: "Home",
  data() {
    return {
      show: false,
    };
  },
  methods: {
    showFlashScreen() {
      this.show = true;
    },
    closeFlashScreen(){
        this.show = false;
    }
  },
};
</script>