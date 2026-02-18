<!-- this is a SFC Single File Component which organise this part of your application in components. This is the parent component-->
<!-- if needed we can pull the subcomponents in here -->
<!-- the script is the JS part -->
<script>
export default {
  data() {
    return {
      words: [
        {
          word_a: "ciao",
          word_b: "hello",
          hint: "greeting",
          answer: "",
          correct: false,
        },
        {
          word_a: "uno",
          word_b: "one",
          hint: "number",
          answer: "",
          correct: false,
        },
        {
          word_a: "grigio",
          word_b: "grey",
          hint: "color",
          answer: "",
          correct: false,
        },
      ],
      correctCount: 0,
      completed: false,
    };
  },
  computed: {
    // this will return the words in a shuffled order
    shuffledWords() {
      return this.words.sort(() => 0.5 - Math.random());
    },
    // this computed property will count the words so that we can enter a message when the words are complete
    //we could not have this line in the data because at that stage the words have not been loaded yet
    wordCount() {
      return this.words.length;
    },
  },
  // this watch is checking that the correct count is euqual to the word count and changing the completed variable to true
  watch: {
    correctCount() {
      this.completed = this.correctCount == this.wordCount;
    },
  },
  methods: {
    checkAnswer(word) {
      word.correct = word.word_b == word.answer;
      // here we are incrementing the correct count if the answer is correct
      if (word.correct) {
        this.correctCount++;
      }
    },
  },
};
</script>

<!-- the template st the HTML part, this is the structure of the component -->
<template>
  <h1>FlashWord</h1>

  <p v-if="completed" id="completed">Great job, you completed all the words!</p>
  <p v-else id="correctCount">
    You have answered {{ correctCount }} out of {{ wordCount }}
  </p>

  <div id="cards">
    <!--the v-bind is adding the class css correct to this dive when the word is correct-->
    <div
      class="card"
      v-for="word in shuffledWords"
      v-bind:class="{ correct: word.correct }"
    >
      <p class="word">{{ word.word_a }}</p>
      <input
        v-if="!word.correct"
        type="text"
        v-model="word.answer"
        v-on:keyup.enter="checkAnswer(word)"
      />
      <p v-else class="correctAnswer">{{ word.answer }}</p>
    </div>
  </div>
</template>

<!-- the style is the CSS part -->
<style>
[v-cloak] {
  display: none;
}

#app {
  /*All these properties are going to be set on the entire div with id = app*/
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: black;
  margin-top: 60px;
}

#cards {
  justify-content: center;
  display: grid;
  grid-template-columns: 300px 300px 300px;
  grid-gap: 30px;
}

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

#correctCount {
  font-size: 20px;
  margin: 10px;
  font-weight: bold;
  padding: 10px;
}

#completed {
  font-size: 20px;
  font-weight: bold;
  color: #0f5132;
  padding: 10px;
  margin: 10px;
}
</style>
