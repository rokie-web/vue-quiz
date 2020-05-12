<template>
    <div class="question-box-container">
        <b-jumbotron>
            <template v-slot:lead>
                <span 
                    v-if="currentQuestion"
                    v-html="currentQuestion.question"
                >
                    {{ currentQuestion.question }}
                </span>

                <span v-else>
                    <center>Congratualtions! You are done!</center>
                </span>
            </template>

            <span v-if="currentQuestion">
                <hr class="my-4">

                <b-list-group>
                    <b-list-group-item button
                        v-for="(answer, index) in shuffledAnswers" 
                        :key="index"
                        :class="answerStyleHandler(index)"
                        @click="selectAnswer(index)"
                    >
                        <span v-html="answer">{{ answer }}</span>
                    </b-list-group-item>
                </b-list-group>

                <b-button 
                    variant="primary"
                    @click="submitAnswer"
                    :disabled="selectedIndex === null || this.answered"
                >
                    Submit
                </b-button>

                <b-button 
                    variant="success" 
                    @click="next"
                >
                    Next
                </b-button>
            </span>
        </b-jumbotron>
    </div>
</template>

<script>
import _ from 'lodash'

export default {
    data: function() {
        return {
            selectedIndex: null,
            shuffledAnswers: [],
            correctIndex: null,
            answered: false
        }
    },
    props: {
        currentQuestion: Object,
        next: Function,
        increment: Function
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
                this.currentQuestion && this.shuffleAnswers()
            }
        }
    },
    methods: {
        selectAnswer(index) {
            this.selectedIndex = index
        },
        shuffleAnswers() {
            let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
            this.shuffledAnswers = _.shuffle(answers)
            this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
        },
        submitAnswer() {
            let isCorrect = false

            if (this.selectedIndex === this.correctIndex) {
                isCorrect = true
            }
            this.answered = true

            this.increment(isCorrect)
        },
        answerStyleHandler(index) {
            let answerStyleClass = ''

            const isIndexSelectedIndex = this.selectedIndex === index
            const isIndexCorrectIndex = this.correctIndex === index

            if (!this.answered && isIndexSelectedIndex) {
                answerStyleClass = 'active'
            } else if (this.answered && isIndexCorrectIndex) {
                answerStyleClass = 'correct'
            } else if (this.answered && this.selectedIndex === index && !isIndexCorrectIndex) {
                answerStyleClass = 'incorrect'
            }

            return answerStyleClass

        }
    }
}
</script>

<style scoped>
    .list-group {
        margin-bottom: 16px;
    }

    .btn {
        margin-right: 6px;
    }

    .correct {
        background-color: rgb(166, 238, 144);
    }

    .incorrect {
        background-color: rgb(248, 129, 129);
    }
</style>