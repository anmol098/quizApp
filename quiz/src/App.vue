<template>
    <div id="app">
        <Header

                :numCorrect="numCorrect"
                :numTotal="numTotal"/>
        <b-container class="bv-example-row">
            <b-row>
                <b-col offset="3" sm="6">
                    <QuestionBox
                            :currentQuestion="questions[index]"
                            :increment="increment"
                            :next="next"
                            v-if="questions.length"
                    />
                </b-col>
            </b-row>
        </b-container>
    </div>
</template>

<script>
    import Header from './components/Header'
    import QuestionBox from './components/QuestionBox'

    export default {
        name: 'app',
        components: {
            Header,
            QuestionBox
        },
        data() {
            return {
                questions: [],
                index: 0,
                numCorrect: 0,
                numTotal: 0
            }
        },
        methods: {
            next() {
                this.index++
            },
            increment(isCorrect) {
                if (isCorrect) {
                    this.numCorrect++
                }
                this.numTotal++
            }
        },
        mounted: function () {
            fetch('https://opentdb.com/api.php?amount=10', {
                method: 'get'
                // eslint-disable-next-line no-unused-vars
            }).then((response) => {
                return response.json()
                // eslint-disable-next-line no-unused-vars
            }).then((jsonData) => {
                this.questions = jsonData.results
            })

        }
    }
</script>

<style>
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }
</style>
