<template>
    <div class="question-box-container">
        <b-jumbotron>
            <template v-slot:lead>
                {{ currentQuestion.question }}
            </template>

            <hr class="my-4">

            <b-list-group>
                <b-list-group-item v-for="answer in answers" :key="answer">
                    {{ answer }}
                </b-list-group-item>
            </b-list-group>

            <b-button variant="primary" href="#">Submit</b-button>

            <b-button variant="success" @click="next">
                Next
            </b-button>
        </b-jumbotron>
    </div>
</template>

<script>
export default {
    props: {
        currentQuestion: Object,
        next: Function
    },
    computed: {
        answers() {
            let answers = [...this.currentQuestion.incorrect_answers]
            answers.push(this.currentQuestion.correct_answer)

            return answers
        }
    }
}
</script>