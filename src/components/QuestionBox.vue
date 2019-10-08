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
        @click="selectAnswer(index)"
        :class="[selectedIndex === index ? 'selected' : '']"
        >
           {{answer}}
         </b-list-group-item>

      </b-list-group>



    <b-button variant="primary" href="#">Submit</b-button>
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
  },
  data() {
    return {
      selectedIndex: null,
      shuffledAnswers: []
    }
  },
  computed:{
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers]
      answers.push(this.currentQuestion.correct_answer)
      return answers
    }
  },
  watch:{
    currentQuestion: {
      immediate:true,
      handler() {
        this.selectedIndex = null
        this.shuffleAnswers()
      }
    }
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index
    },
    shuffleAnswers(){
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
      this.shuffledAnswers = _.shuffle(answers)
    }
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
