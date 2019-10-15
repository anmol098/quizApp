<template>
    <div class="question-box">
        <b-jumbotron>
            <template v-slot:lead>
                {{ currentQuestion.question }}
            </template>

            <hr class="my-4">
            <b-list-group>
                <b-list-group-item
                        :class="answerClass(index)"
                        :key="index"
                        @click="selectAnswer(index)"
                        v-for="(answer, index) in answers">{{answer}}
                </b-list-group-item>

            </b-list-group>

            <b-button
                    :disabled="selectedIndex === null || answered"
                    @click="submitAnswer"
                    variant="primary"

            >Submit
            </b-button>
            <b-button @click="next" href="#" variant="success">Next</b-button>
        </b-jumbotron>
    </div>
</template>

<script>
    import _ from 'lodash'

    export default {
        props: {
            currentQuestion: Object,
            next: Function,
            increment: Function
        },
        computed: {
            answers() {
                let answers = [...this.currentQuestion.incorrect_answers];
                answers.push(this.currentQuestion.correct_answer);
                return answers
            }
        },
        watch: {
            currentQuestion: {
                immediate: true,
                handler() {
                    this.selectedIndex = null;
                    this.answered = false;
                    this.shuffleAnswer()
                }
            }
        },
        methods: {
            selectAnswer(index) {
                this.selectedIndex = index
            },
            shuffleAnswer() {
                let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer];
                this.shuffledAnswers = _.shuffle(answers);
                this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)


            },
            submitAnswer() {
                let isCorrect = false;
                if (this.selectedIndex === this.correctIndex) {
                    isCorrect = true
                }
                this.answered = true;
                this.increment(isCorrect)
            },
            answerClass(index) {
                let answerClass = '';

                if (!this.answered && this.selectedIndex === index)
                    answerClass = 'selected';
                else if (this.answered && this.correctIndex === index)
                    answerClass = 'correct';
                else if (this.answered && this.selectedIndex === index && this.correctIndex !== index)
                    answerClass = 'incorrect';
                return answerClass
            }
        },
        data() {
            return {
                selectedIndex: null,
                shuffledAnswers: [],
                correctIndex: null,
                answered: false,
            }
        }

    }
</script>

<style scoped>
    .list-group-item:hover {
        background: #eeeeee;
        cursor: pointer;
    }

    .list-group {
        margin-bottom: 15px;
    }

    .btn {
        margin: 0 5px;
    }

    .selected {
        background: lightblue;
    }

    .correct {
        background: lightgreen;
    }

    .incorrect {
        background: red;
    }
</style>

