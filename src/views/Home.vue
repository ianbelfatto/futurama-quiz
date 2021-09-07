<template>
  <div class="d-flex justify-content-center">
    <div v-if="showScore">
      <b-card title="Results" style="max-width: 20rem">You Scored {{ score }} of {{ questions.length }}</b-card>
    </div>
    <div class="card-q" v-else>
      <span v-if="!startQuiz">
        <b-card title="Futurama Quiz" style="max-width: 20rem" class="mb-2">
          <b-card-text></b-card-text>
          <b-button @click="startQuizFunc()">Start Quiz</b-button>
        </b-card>
      </span>
      <span v-else>
        <b-card title="Simple Quiz Application" style="max-width: 20rem" class="mb-2">
          <b-card-text>Question No.{{ currentQuestion + 1 }} of {{ questions.length }}</b-card-text>
          <br />
          <b-progress variant="warning" :max="30" :value="countDown" height="4px"></b-progress>

          <b-card-text>
            <span style="font-size: 40px">
              <strong>{{ countDown }}</strong>
            </span>
          </b-card-text>
          <div>
            {{ questions[currentQuestion].questionImage }}
          </div>
          <b-card-text>
            {{ questions[currentQuestion].questionText }}
          </b-card-text>
          <div class="answer-section">
            <b-button
              :key="index"
              v-for="(option, index) in questions[currentQuestion].answerOptions"
              @click="handleAnswerClick(option.isCorrect)"
              class="ans-option-btn"
              variant="primary"
            >
              {{ option.answerText }}
            </b-button>
          </div>
        </b-card>
      </span>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currentQuestion: 0,
      showScore: false,
      score: 0,
      countDown: 30,
      timer: null,
      startQuiz: false,

      questions: [
        {
          questionImage: "https://i.imgur.com/YKqeyhv.gif",
          questionText: "What is Bender's full name?",
          answerOptions: [
            { answerText: "Bender Robot Rodriguez", isCorrect: false },
            { answerText: "Bender Bending Rodriguez", isCorrect: true },
            { answerText: "Bender Fender Rodriguez", isCorrect: false },
            { answerText: "Bender Roberto Rodriguez", isCorrect: false },
          ],
        },
        {
          questionText: "How many years did Seymour the Dog wait for Fry outside Panucci's Pizza?",
          answerOptions: [
            { answerText: "4", isCorrect: false },
            { answerText: "8", isCorrect: false },
            { answerText: "12", isCorrect: true },
            { answerText: "15", isCorrect: false },
          ],
        },
        {
          questionText: "What is Fry's PIN number?",
          answerOptions: [
            { answerText: "1577", isCorrect: false },
            { answerText: "1077", isCorrect: true },
            { answerText: "1055", isCorrect: false },
            { answerText: "1177", isCorrect: false },
          ],
        },
        {
          questionText: "What is the name of Leela's old martial arts teacher?",
          answerOptions: [
            { answerText: "Master Fnaw", isCorrect: true },
            { answerText: "Master Fnog", isCorrect: false },
            { answerText: "Master Fnork", isCorrect: false },
            { answerText: "Master Fnark", isCorrect: false },
          ],
        },
        {
          questionText: "In 'Parasite's Lost', what does Fry eat that gives him super-human abilities?",
          answerOptions: [
            { answerText: "Egg Salad Sandwich", isCorrect: true },
            { answerText: "Turkey Club Sandwich", isCorrect: false },
            { answerText: "Double Bacon Cheeseburger", isCorrect: false },
            { answerText: "Bachelor Chow", isCorrect: false },
          ],
        },
        {
          questionText: "What planet does Santa Claus live on?",
          answerOptions: [
            { answerText: "Mars", isCorrect: false },
            { answerText: "Mercury", isCorrect: false },
            { answerText: "Jupiter", isCorrect: false },
            { answerText: "Neptune", isCorrect: true },
          ],
        },
        {
          questionText: "In 'Bednin' in the Wind' which musician does Bender bring on tour with him?",
          answerOptions: [
            { answerText: "Bob Dylan", isCorrect: false },
            { answerText: "Bob Weir", isCorrect: false },
            { answerText: "Billy Idol", isCorrect: false },
            { answerText: "Beck", isCorrect: true },
          ],
        },
      ],
    };
  },

  methods: {
    startQuizFunc() {
      this.startQuiz = true;
      this.countDownTimer();
    },
    handleAnswerClick(isCorrect) {
      clearTimeout(this.timer);
      let nextQuestion = this.currentQuestion + 1;
      if (isCorrect) {
        this.score = this.score + 1;
      }
      if (nextQuestion < this.questions.length) {
        this.currentQuestion = nextQuestion;

        this.countDown = 30;
        this.countDownTimer();
      } else {
        this.showScore = true;
      }
    },
    countDownTimer() {
      if (this.countDown > 0) {
        this.timer = setTimeout(() => {
          this.countDown -= 1;
          this.countDownTimer();
        }, 1000);
      } else {
        this.handleAnswerClick(false);
      }
    },
  },
  created() {},
};
</script>

<style scoped>
.card {
  min-width: 100%;
  border-radius: 15px;
  padding: 20px;
  /* box-shadow: 10px 10px 42px 0px rgba(0, 0, 0, 0.75); */
}
.card-q {
  min-width: 60%;
}
.ans-option-btn {
  min-width: 50%;
  font-size: 16px;
  color: #ffffff;
  align-items: center;
  cursor: pointer;
  margin-bottom: 5px;
}
.answer-section {
  width: 100%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
.timer-text {
  background: rgb(230, 153, 12);
  padding: 15px;
  margin-top: 20px;
  margin-right: 20px;
  border: 5px solid rgb(255, 189, 67);
  border-radius: 15px;
  text-align: center;
}

.card-img,
.card-img-top {
  border-top-left-radius: calc(0.25rem - 1px);
  border-top-right-radius: calc(0.25rem - 1px);
  height: 350px;
}
</style>
