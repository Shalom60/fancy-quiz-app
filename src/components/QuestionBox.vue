<template>
    <div class="question-box-container">
  <b-jumbotron v-if="numTotal<10">
    <template v-slot:header> Question
    {{numTotal+1}}
    </template>

    <template v-slot:lead>
     {{CurrentQuestion.question}}
    </template>

    <hr class="my-4">

    <b-list-group>
      <b-list-group-item v-for="(answer, index) in shuffledAnswers" :key="index"  @click.prevent="selectAnswer(index)"
      :class="answerClass(index)"
      >
      {{answer}}
      </b-list-group-item>
      
    </b-list-group>

    <p >
      
    </p>

    <b-button variant="primary"  @click="submitAnswer" 
    :disabled="selectedIndex === null || answered"
     >Submit
     </b-button>

    <b-button @click= "next" variant="success"
    :disabled="!answered"
    >Next</b-button>
  </b-jumbotron>

  <b-jumbotron v-else class="final">
       <template v-slot:lead  > Final score is
     {{numCorrect}}/{{numTotal}}
    </template> 
  </b-jumbotron>
 
</div>
</template>

<script>
import _ from 'lodash' 
  
export default {
  props: {
    CurrentQuestion: Object,
    next: Function, 
    increment: Function,
    numTotal: Object,
    numCorrect: Object
  },
  data() {
    return{
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      answered: false,
      
    }
  },
  computed: {
    answers() {
      let answers = [...this.CurrentQuestion.incorrect_answers]
      answers.push(this.CurrentQuestion.correct_answer)
      return answers
    },
  },
  watch: {
    CurrentQuestion: {
      immediate: true, 
      handler() {
        this.selectedIndex = null
        this.answered = false
      this.shuffleAnswers()     
      }
    }
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index
          },
    shuffleAnswers() {
      let answers =  [...this.CurrentQuestion.incorrect_answers, this.CurrentQuestion.correct_answer]
      this.shuffledAnswers = _.shuffle(answers)
      this.correctIndex = this.shuffledAnswers.indexOf(this.CurrentQuestion.correct_answer)
      
    },
    submitAnswer() {
      let isCorrect = false
      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true
      }
      this.answered = true
      this.increment(isCorrect)
    },
    answerClass(index) {
      let answerClass = ''
        !this.answered && this.selectedIndex === index ? answerClass = 'selected' : 
        this.answered && this.correctIndex === index ? answerClass ='correct' : 
        this.answered && this.selectedIndex=== index && this.correctIndex !== index ? answerClass ='incorrect' : ''
        return answerClass
    }
  },

}
</script>

<style scoped>
.list-group {
  margin-bottom: 15px;
}
.list-group-item:hover {
  background: grey;
  cursor: pointer;
}
.btn {
  margin: 5px;
}

.selected{
  background: lightblue;
}

.correct{
  background-color: lightgreen;
}

.incorrect{
  background-color: red;
}
.final{
   font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
   font-size: 120px;
   color: #552382;
}
</style>