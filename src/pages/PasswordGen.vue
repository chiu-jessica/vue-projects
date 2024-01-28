<template>
    <div id = "password-gen">
        <form> 
            <div id = "password-container">
                <input v-model = "password" id = "box" :type = "(displayPassword)?'text':'password'"/>
                <span class="material-symbols-outlined eye" @click = "displayPassword = true" v-if = "!displayPassword">visibility</span>
                <span class="material-symbols-outlined eye" @click = "displayPassword = false" v-else>visibility_off</span>
            </div>
            <div id = "check-boxes">
                <input v-model="options.letters" type="checkbox"/>
                <label>Letters</label>
                <input v-model="options.numbers" type="checkbox"/>
                <label>Numbers</label>
                <input v-model="options.symbols" type="checkbox"/>
                <label>Symbols</label>
            </div>
            <div class = "slider-container">
                <p>Character Count: {{ characterCount }}</p>
                <input class = "character-length" type="range" v-model = "characterCount" min = "1" max = "20"/>
            </div>
            <div class = "button-container">
                <button @click.prevent = "generatePassword()">Generate Password</button>
            </div>
        </form>
    </div>
</template>

<script>

    export default {
        data() {
            return {
                displayPassword: false,
                characterCount: 1,
                letterStr: "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz",
                symbolStr: "!@#$%^&*()<>?{}-+=",
                password: "",
                options: {
                    letters: false,
                    numbers: false,
                    symbols: false
                }
            };
        },
        methods: {
            randomCategory() {
                var categoryArr = []
                for (const category in this.options) {
                    if (this.options[category]) {
                        categoryArr.push(category)
                    }
                }
                const randomIndex =  Math.floor(Math.random()*categoryArr.length)
                return categoryArr[randomIndex]
            },
            generatePassword() {
                this.password = ""
                var passwordIndex = 0
                while (passwordIndex < this.characterCount) {
                    const category = this.randomCategory()
                    if (category == "letters") {
                        this.password += this.randomLetter()
                    } else if (category == "numbers") {
                        this.password += this.randomNumber()
                    } else if (category == "symbols") {
                        this.password += this.randomSymbol()
                    } else {
                        return 
                    }
                    passwordIndex++
                }
            },
            randomLetter() {
                const letterIndex = Math.floor(Math.random()*this.letterStr.length)
                return this.letterStr.substring(letterIndex, letterIndex+1)
            },
            randomNumber() {
                const numberIndex = Math.floor(Math.random()*10)
                return numberIndex
            },
            randomSymbol() {
                const symbolIndex = Math.floor(Math.random()*this.symbolStr.length)
                return this.symbolStr.substring(symbolIndex,symbolIndex+1)
            }
        }
    }
</script>

<style lang = "scss">
    #password-gen {
        background-color: #ffc7d2;
        width: 100%;
        height: 100vh;
        display: flex;
        align-items: center;
        justify-content: center;
        #password-container {
            position: relative;
            width: 100%;
            input {
                width: 100%;
            }
            .eye {
                position: absolute;
                right: 0px;
                top: 50%;
                transform: translateY(-50%);
                font-size: 17px;
                cursor: pointer;
            }
        }
        .character-length {
            -webkit-appearance: none;
            &::-webkit-slider-runnable-track {
                width: 300px;
                height: 5px;
                background: #f389b5;
                border: none;
                border-radius: 3px;
            }
            &::-webkit-slider-thumb {
                -webkit-appearance: none;
                border: none;
                height: 14px;
                width: 14px;
                border-radius: 50%;
                background: #ba2060;;
                margin-top: -4px;
            }
            &:focus {
                outline: none;
                &::-webkit-slider-runnable-track {
                    background: #d46895;
                }
            }
        }
        .slider-container {
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        .button-container {
            display: flex;
            justify-content: center;
        }
        button {
            margin-top: 20px;
            padding: 7px;
            background-color:#f389b5;
            border: 2px solid #ba2060;
            border-radius: 7px;
            color: #790636;
        }
    }
</style>