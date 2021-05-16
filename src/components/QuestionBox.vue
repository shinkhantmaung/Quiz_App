<template>
  <a-card>
    <h1 style="fontSize: 20px;color: rgba(0, 0, 0, 0.85); marginBottom: 16px;fontWeight: 500">
      {{ currentQuestion.question }}
    </h1>
    <a-list bordered :data-source="data">
      <a-list-item  
        v-for="(answer, index) in shuffledAnswers"
        :key="index"
        @click.prevent="selectAnswer(index)"
        :class="answerClass(index)">
        {{index+1}}. {{ answer }}
      </a-list-item>
    </a-list><br>
    <a-button type="primary" :size="size" style="margin:10px;" 
        @click="submitAnswer"
        :disabled="selectedIndex === null || answered"
    >
      Submit
    </a-button>
    <a-button :size="size" style="margin:10px;" @click="next" variant="success">
        Next <a-icon type="right" />
    </a-button> 
  </a-card>
</template>



<script>
import _ from 'lodash'

export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function
  },
  data: function() {
    return {
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      answered: false,
      size: 'large'
    }
  },
  computed: {
    answers() {
      // this function is no longer used in finished code
      // it is replaced by the watch function below and the
      // shuffleAnswers method
      let answers = [...this.currentQuestion.incorrect_answers]
      answers.push(this.currentQuestion.correct_answer)
      return answers
    }
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null
        this.answered = false
        this.shuffleAnswers()
      }
    }
  },
  methods: {
    handleSizeChange(e) {
      this.size = e.target.value;
      },
    selectAnswer(index) {
      this.selectedIndex = index
    },
    submitAnswer() {
      let isCorrect = false

      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true
      }
      this.answered = true

      this.increment(isCorrect)
    },
    shuffleAnswers() {
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
      this.shuffledAnswers = _.shuffle(answers)
      this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
    },
    answerClass(index) {
      let answerClass = ''

      if (!this.answered && this.selectedIndex === index) {
        answerClass = 'selected'
      } else if (this.answered && this.correctIndex === index) {
        answerClass = 'correct'
      } else if (this.answered &&
        this.selectedIndex === index &&
        this.correctIndex !== index
      ) {
        answerClass = 'incorrect'
      }

      return answerClass
    }
  }
}
</script>

<style scoped>
.a-list {
  margin-bottom: 15px;
}

.a-list-item:hover {
  background: #333;
  cursor: pointer;
}

.a-button {
  margin: 0 5px;
}

.selected {
  background-color: lightblue;
}

.correct {
  background-color: lightgreen;
}

.incorrect {
  background-color: red;
}
</style>
