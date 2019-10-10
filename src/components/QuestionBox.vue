<template>
  <div>
    <b-jumbotron>

    <template v-slot:lead>
      {{currentQuestion.question}}
    </template>

    <hr class="my-4">

      <b-list-group>
        <b-list-group-item
        v-for="(answer, index) in answers"
        :key="index"
        @click.prevent="selectAnswer(index)"
        :class="[selectedIndex === index ? 'selected' : '']"

          >
          {{answer}}
         </b-list-group-item>

      </b-list-group>



    <b-button variant="primary"
      @click = "submitAnswer"
      :disabled="!selectedIndex"
      >
      Submit
    </b-button>

    <b-button @click="next" variant="success" href="#">Next</b-button>
  </b-jumbotron>
  </div>
</template>

<script>
import _ from 'lodash'

export default{
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function,
  },
  data() {
    return {
      selectedIndex: null,
      shuffledAnswers: [],
      correctIndex: null
    }
  },
  computed:{
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers]
      answers.push(this.currentQuestion.correct_answer)
      answers = _.shuffle(answers)
      this.correctIndex = answers.indexOf(this.currentQuestion.correct_answer)
      return answers
    }
  },

  methods: {
    selectAnswer(index) {
      this.selectedIndex = index
    },

    submitAnswer(){
        let isCorrect = false;
       if(this.selectedIndex == this.correctIndex){
         isCorrect = true;
       }
       this.increment(isCorrect)
       this.selectedIndex = null

    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.list-group, .btn{
  margin:15px;
}

.list-group-item{
  margin-bottom: 10px;
}

.list-group-item:hover{
  background-color: #eeeeee;
  cursor:pointer;
}

.selected{
  background-color:lightblue;
}

.correct{
  background-color:green;
}
.incorrect{
  background-color:red;
}

</style>
