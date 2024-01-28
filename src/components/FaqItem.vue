<template>
    <div class="faqItem"> 
        <div class="question">
            <h2>{{faqObject.question}}</h2>
            <span :class="{'isOpen':isOpen}" class="material-symbols-outlined clickable arrow" @click="open">expand_more</span>
        </div>
        <transition name="slide-down" mode="in-out">
            <div class="answer" v-if="isOpen">
                <p>{{faqObject.answer}}</p>
            </div>
        </transition>
    </div>
</template>

<script>
    export default{
        props: {
            faqObject: {
                type: Object, 
                required: true
            }
        },
        data() {
            return {
                isOpen: false
            }
        },
        methods: {
            open() {
                this.isOpen = !this.isOpen
            }
        }
    }
</script>

<style lang = "scss">
    .faqItem {
        width: 500px;
        margin: 10px auto;
        display: flex;
        flex-direction: column;
        .question {
            display: flex;
            align-items: center;
            justify-content: space-between;
            background-color: #3f8efc;
            padding: 0px 15px;
            color: white;
            z-index: 2;
        }
        .answer {
            background-color: #add7f6;
            z-index: 1;
        }
        .clickable {
            cursor: pointer;
        }
        .arrow {
            transition: all 1s;
        }
        .isOpen {
            transform: rotate(180deg);
        }
        .slide-down-enter-active, 
        .slide-down-leave-active {
            transition: all 1s;
        }
        .slide-down-enter-from,
        .slide-down-leave-to {
            transform: translateY(-100%);
        }
    }
</style>