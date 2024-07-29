<template>
  <div class="container" v-if="questions.length">
    <div v-if="!showResult">
      <div class="question-box">
        <div class="question" v-html="currentQuestion.question"></div>
        <div class="answers">
          <button
            v-for="answer in shuffledAnswers"
            :key="answer"
            @click="checkAnswer(answer)"
          >
            {{ answer }}
          </button>
        </div>
        <div class="question-count">
          {{ currentQuestionIndex + 1 }}/{{ questions.length }}
        </div>
      </div>
    </div>
    <div v-else class="result-box">
      <p>
        You got {{ correctAnswers }} out of {{ questions.length }} questions
        correct!
      </p>
      <button @click="retry">Retry</button>
    </div>
  </div>
  <div v-else class="loading">Loading questions...</div>
</template>

<script>
export default {
  data() {
    return {
      questions: [],
      currentQuestionIndex: 0,
      correctAnswers: 0,
      showResult: false,
    };
  },
  computed: {
    currentQuestion() {
      return this.questions[this.currentQuestionIndex] || {};
    },
    shuffledAnswers() {
      if (!this.currentQuestion || !this.currentQuestion.correct_answer)
        return [];
      const answers = [
        this.currentQuestion.correct_answer,
        ...this.currentQuestion.incorrect_answers,
      ];
      return answers.sort(() => Math.random() - 0.5);
    },
  },
  methods: {
    fetchQuestions() {
      fetch("https://opentdb.com/api.php?amount=10")
        .then((response) => response.json())
        .then((data) => {
          this.questions = data.results;
        })
        .catch((error) => {
          console.error("Error fetching questions:", error);
        });
    },
    checkAnswer(answer) {
      if (answer === this.currentQuestion.correct_answer) {
        this.correctAnswers++;
      }
      this.currentQuestionIndex++;
      if (this.currentQuestionIndex >= this.questions.length) {
        this.showResult = true;
      }
    },
    retry() {
      this.questions = [];
      this.currentQuestionIndex = 0;
      this.correctAnswers = 0;
      this.showResult = false;
      this.fetchQuestions();
    },
  },
  mounted() {
    this.fetchQuestions();
  },
};
</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 100%;
  max-width: 600px;
  margin: 0 auto;
  background-color: #e0f7fa;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  text-align: center;
}

.question-box,
.result-box {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.question {
  font-size: 32px;
  margin-bottom: 20px;
}

.answers {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  width: 100%;
}

.answers button {
  background: #ffffff;
  border: 1px solid #ccc;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
  margin: 5px;
  flex: 1 1 calc(50% - 10px); /* Adjust button size and spacing */
}

.answers button:hover {
  background: #f0f0f0;
}

.question-count {
  margin-top: 20px;
  font-size: 14px;
}

.result-box p {
  font-size: 20px;
  margin-bottom: 20px;
}

.result-box button {
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
}

.loading {
  text-align: center;
  font-size: 18px;
  margin-top: 20px;
}
</style>
