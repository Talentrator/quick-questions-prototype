<template>
  <div
    class="position-absolute top-0 bottom-0 w-100 flash-screen left-0 bg-white"
  >
    <question-card />
  </div>
</template>

<script>
import QuestionCard from "./QuestionCard.vue";
export default {
  components: { QuestionCard },
  name: "QuestionCards",
  emits: ["close"],
  methods: {},
};
</script>
<style lang="scss">
.flash-screen {
  z-index: 1000;
}
</style>