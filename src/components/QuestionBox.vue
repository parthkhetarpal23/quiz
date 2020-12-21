<template>
    <div>
        <b-jumbotron>
           

            <template #lead>
            {{ currentQuestion.question }}
            </template>
            <hr>
           <b-list-group>
                <b-list-group-item 
                    v-for= "(answer,index) in answers" 
                    :key= "index"
                    @click.prevent= "selectAnswer(index)"
                    :class = "[selectedIndex === index  ? 'selected' : '']"
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
export default {
    props: {
        currentQuestion: Object, //  Any var (data property or method that you're passing from app to questionbox inorder for it to be displayed in html needs to be referenced in props (receiving this var from parent) )
        next: Function
    },
    data(){
        return {
            selectedIndex : null
        }
    },
    computed: {
        answers() {
        let answers = [...this.currentQuestion.incorrect_answers]
        answers.push(this.currentQuestion.correct_answer)
        return answers
        }
    },
    watch : {
      currentQuestion() {
          this.selectedIndex = null;
          this.shuffleAnswers()
      }
    },
    methods: {
        selectAnswer(index){
            this.selectedIndex = index
            
        },
        shuffleAnswers() {
          
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