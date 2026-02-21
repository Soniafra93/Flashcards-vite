<script>
export default {
  name: "WordCard",
  // props are passing data from a parent to a child component, an event is passing data from the child to the parent component
  props: ["word"],
  methods: {
    checkAnswer() {
      this.word.correct = this.word.word_b == this.word.answer;
      // here we are incrementing the correct count if the answer is correct
      if (this.word.correct) {
        this.correctCount++;
        this.$emit("incrementCorrectCount");
      }
    },
  },
};
</script>

<template>
  <!--the v-bind is adding the class css correct to this dive when the word is correct-->
  <div class="card" v-bind:class="{ correct: word.correct }">
    <p class="word">{{ word.word_a }}</p>
    <input
      v-if="!word.correct"
      type="text"
      v-model="word.answer"
      v-on:keyup.enter="checkAnswer()"
    />
    <p v-else class="correctAnswer">{{ word.answer }}</p>
  </div>
</template>

<style scoped>
/* the scoped in the style file has the function to make this style only apply to content inside the single file component */
.card {
  background-color: #e8f0ff;
  border-radius: 5px;
  padding: 10px 0;
  font-size: 25px;
}

input[type="text"] {
  border: 0;
  font-size: 25px;
  border-radius: 5px;
  margin-top: 5px;
  text-align: center;
  padding: 5px;
}

.word {
  font-weight: bold;
}

.correctAnswer {
  padding: 0;
  margin: 0;
}

.correct {
  color: #0f5132;
  background-color: #d1e7dd;
}
</style>
