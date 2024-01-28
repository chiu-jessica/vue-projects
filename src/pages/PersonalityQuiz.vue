<template>
    <div id = "personality-quiz">
        <transition name = "fade" mode = "out-in">
            <div class = "question-wrapper" v-if = "activeQuestion" v-show="!inTransition">
                <div class = "question">
                    <h2>{{activeQuestion.question}}</h2>
                </div>
                <div class = "options-table">
                    <div class = "option-button" :style = "{backgroundColor: (answer==activeQuestion.userAnswer)?'#9bbac7':'#c6ae89'}" @click = "handleClick(answer)" v-for = "(answer, aindex) in activeQuestion.answers" :key="qindex + 'answer' + aindex">{{answer.name}}</div>
                </div>
            </div>
        </transition>
        <div class = "quiz-actions">
            <div @click="prev" class="left-arrow">
                    <span class="material-symbols-outlined">chevron_left</span>
            </div>
            <div class = "dots">
                <div class = "dot" v-for = "index in questions.length" :key = "index">
                    <span class="material-symbols-outlined" v-if = "index-1 == questionIndex"> radio_button_checked </span>
                    <span class="material-symbols-outlined" v-else> radio_button_unchecked </span>
                </div>
            </div>
            <div @click="next" class="right-arrow">
                    <span class="material-symbols-outlined">chevron_right</span>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data () {
            return {
                inTransition: false,
                questions: [
                    {
                        question: "1. What is your favorite food?",
                        answers: [{name:'pizza', x:1, y:1}, {name:'sushi', x:1, y:-1}, {name:'salad', x:-1, y:-1}, {name:'hamburgers', x:-1, y:1}],
                        userAnswer: ""
                    },
                    {
                        question: "2. What is your favorite color?",
                        answers: [{name:'green', x:1, y:-1}, {name:'blue', x:-1, y:1}, {name:'black', x:-1, y:-1}, {name:'red', x:1, y:1},],
                        userAnswer: ""
                    },
                    {
                        question: "3. What is your favorite season?",
                        answers: [{name:'spring', x:1, y:-1}, {name:'winter', x:-1, y:1}, {name:'summer', x:1, y:1}, {name:'fall', x:-1, y:-1}],
                        userAnswer: ""
                    }
                ],
                questionIndex: 0,
                activeQuestion: null
            }
        },
        watch: {
            questionIndex (val) {
                this.inTransition = true
                setTimeout(() => {
                    this.inTransition = false
                    this.activeQuestion = this.questions[val]
                }, 1000);
            },
            quizComplete (val) {
                if (val) {
                    location.href = "/quiz-result?result=" + this.submit()
                }
            }
        },
        methods: {
            prev () {
                if (this.questionIndex > 0) {
                    this.questionIndex--;
                }
            },
            next () {
                if (this.questionIndex < this.questions.length-1) {
                    this.questionIndex++;
                }
            },
            handleClick (answer) {
                this.questions[this.questionIndex].userAnswer = answer
                this.next()
            },
            submit () {
                const x = this.userCoordinates.x
                const y = this.userCoordinates.y
                if (x<=0 && y<0) {
                    return "Drax"
                } else if (x<0 && y>=0) {
                    return "Rocket"
                } else if (x>=0 && y>0) {
                    return "Starlord"
                } else if (x>0 && y<=0) {
                    return "Gamora"
                } else {
                    return "NPC"
                }
            }
        },
        mounted () {
            this.activeQuestion = this.questions[0]
        },
        computed: {
            userCoordinates () {
                var coord = {x:0, y:0}
                for (const item of this.questions) {
                    if (item.userAnswer) {
                        coord.x += item.userAnswer.x
                        coord.y += item.userAnswer.y
                    }
                }
                return coord
            },
            quizComplete () {
                for (const item of this.questions) {
                    if (!item.userAnswer) {
                        return false
                    }
                }
                return true
            }
        }
    }
</script>

<style lang="scss">
@import url(https://fonts.googleapis.com/css?family=Castoro);
    html, body {
        margin: 0;
    }
    #personality-quiz {
        position: relative;
        background-color:#90c796;
        height: 100vh;
        width: 100vw;
        display: flex;
        justify-content: center;
        align-items: center;
        .quiz-actions {
            position: absolute;
            top: 20px;
            display: flex;
            align-items: center;
            color: #39463a;
            cursor: pointer;
            .material-symbols-outlined {
                font-size: 30px;
            }
            .dots {
                display: flex;
            }
        }
        .question-wrapper {
            width: 700px;
            height: 400px;
            background-color: white;
            border-radius: 30px;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            padding: 20px;
            .question {
                font-family: 'Playfair Display', serif;
                color: #465447;
            }
            .options-table {
                width: 100%;
                display: grid;
                grid-template-columns: 1fr 1fr;
                grid-gap: 40px;
                .option-button {
                    width: 100%;
                    height: 60px;
                    background-color: #c6ae89;
                    border-radius: 15px;
                    display: flex;
                    align-items: center;
                    justify-content: center;
                    color: #39463a; 
                    font-size: 22px;
                    font-family: 'Castoro', serif;
                    cursor: pointer;
                }
            }
        }
    }
    .fade-enter-active, .fade-leave-active {
        transition: opacity 1s ease-in-out !important;
    }
    .fade-enter-from, .fade-leave-to {
        opacity: 0 !important;
    }
    .fade-enter-to, .fade-leave-from {
        opacity: 1 !important;
    }
</style>