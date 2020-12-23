<template>
    <div>
        <b-jumbotron>
           

            <template #lead>
            {{ currentQuestion.question }}
            </template>
            <hr>
           <b-list-group>
                <b-list-group-item 
                    v-for= "(answer,index) in shuffledAnswers" 
                    :key= "index"
                    @click.prevent= "selectAnswer(index)"
                    :class = "answeredClass(index)"
                    >
                    {{answer}}
                </b-list-group-item>
            </b-list-group>
            <b-button 
            variant= "primary"
            @click= "submitAnswer"
            :disabled= "selectedIndex === null || answered"
            >
            Submit</b-button>
            <b-button 
            variant= "primary"
            @click= "next"
            
            
            >Next</b-button>
        </b-jumbotron>
</div>

</template>
<script>
import _ from 'lodash'
export default {
    props: {
        currentQuestion: Object, //  Any var (data property or method that you're passing from app to questionbox inorder for it to be displayed in html needs to be referenced in props (receiving this var from parent) )
        next: Function,
        increment: Function,
        
        
        
    },
    data(){
        return {
            selectedIndex: null,
            correctIndex: null,
            shuffledAnswers: [],
            answered: false
        }
    },
    computed: {
        answers() {
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
        selectAnswer(index){
            this.selectedIndex = index
            
        },
        shuffleAnswers() {
          let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
          this.shuffledAnswers = _.shuffle(answers)
           this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)

        },
        submitAnswer(){
            let isCorrect = false

            if(this.selectedIndex === this.correctIndex){
                isCorrect = true
            }
            this.answered = true
            this.increment(isCorrect,this.answered)
        },
        answeredClass(index){
            let answerClass = ''
            if(!this.answered && this.selectedIndex === index){
                answerClass = 'selected'
            }
            else if( this.answered && this.correctIndex === index){
                answerClass = 'correct'
            }
            else if( this.answered && this.selectedIndex === index && index != this.correctIndex){
                answerClass = 'incorrect'
            }
           return answerClass

        }
    }
}
</script>
<style scoped>
.btn {
    margin:70px;
}
.list-group-item:hover {
    background: gray
}
.selected {
    background-color: lightblue;
}
.correct {
    background-color: greenyellow;
}
.incorrect {
    background-color: red;
}
</style>