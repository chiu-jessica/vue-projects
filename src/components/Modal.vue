<template>
    <div class = "modal-mask">
        <div class = "modal-wrapper">
            <div class = "modal-container">
                <div class = "close" @click = "close">
                    <span class="material-symbols-outlined">cancel</span>
                </div>
                <slot>
                   test 
                </slot>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "ModalVue",
        methods: {
            close () {
                this.$emit("close");
            }
        }
    }
</script>

<style lang = "scss">
    .modal-mask {
        position: fixed;
        z-index: 1000;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background-color:rgba(0, 0, 0, 0.3);
        display: table;
        .modal-wrapper {
            display: table-cell;
            vertical-align: middle;
            .modal-container {
                width: 400px;
                margin: 0 auto;
                background-color: white;
                padding: 20px;
                border-radius: 5px;
                box-shadow: 0 4px 8px rgba(0, 0, 0, .4);
                position: relative;
                .close {
                    position: absolute;
                    top: 5px;
                    right: 5px;
                    cursor: pointer;
                }
            }
        }
    }
</style>