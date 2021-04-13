<template>
    <div>
        <div>
        <b-jumbotron header="Take the quiz">
             <b-jumbotron>
                <template v-slot:lead>
                    {{ currentQuestion.question }}
                </template>

                <hr class="my-4">
                <b-list-group>
                    <b-list-group-item 
                        v-for="(answer, index) in answers" 
                        :key="index"
                        @click="selectAnswer(index)"
                        :class="answerClass(index)"
                        >
                            {{ answer }}
                    </b-list-group-item>
                </b-list-group>
                <br>
                <b-button 
                    variant="primary" 
                    @click="submitAnswer"
                    :disabled="selectIndex === null || answered"
                    >Answer Question</b-button>
                <b-button variant="success" @click="nextQuestion">Next Question</b-button>
            </b-jumbotron>
        </b-jumbotron>
        </div>
    </div>
</template>
<script>
import _ from 'lodash'
export default {
    props : {
        currentQuestion : Object,
        nextQuestion : Function,
        increment : Function
    },
    data(){
        return {
            selectIndex : null,
            correctIndex : null,
            randomAnswers : [],
            answered : false
        }
    },
    computed : {
        answers(){
            let answers = [...this.currentQuestion.incorrect_answers]
            answers.push(this.currentQuestion.correct_answer)
            return answers
        }
    },
    watch : {
        currentQuestion : {
            immediate : true,
            handler(){
                this.selectIndex = null
                this.answered = false
                this.shuffleAnswers()
            }
        }
    },
    methods : {
        selectAnswer(index){
            this.selectIndex = index
        },
        shuffleAnswers(){
            let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
            this.randomAnswers = _.shuffle(answers)
            this.correctIndex = this.randomAnswers.indexOf(this.currentQuestion.correct_answer)
        },
        submitAnswer(){
            let isCorrect = false
            console.log(this.selectIndex, this.correctIndex)
            if(this.selectIndex === this.correctIndex){ 
                isCorrect = true
             }
            this.answered = true
            this.increment(isCorrect)

        },
        answerClass(index){

            let answerClass = ''
            if(!this.answered && this.selectIndex === index) {answerClass = 'selected'}
            else if(this.answered && index === this.correctIndex ){ answerClass = 'correct'}
            else if(this.answered && this.selectIndex === index && this.correctIndex !== index) {answerClass = 'incorrect'}

            return answerClass
        }
    }
}
</script>

<style scoped>
.list-group {
    margin-bottom: 15px;
}
.list-group-item:hover {
    background-color: #eee;
    cursor: pointer;
}
.selected {
    background-color: aqua;
}
.correct {
    background-color: greenyellow;
}
.incorrect {
    background-color: red;
}
.btn {
    margin: 0 5px;
}
</style>