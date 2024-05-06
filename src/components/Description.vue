<template>
    <input @input="changeDescription" v-model="description" type="text" id="description" name="description">
    <span v-show="balance">The text is balanced.</span>
    <span v-show="notBalance">The text is not balanced.</span>
</template>


<script>
import { defineComponent } from 'vue'
export default defineComponent({
    data() {
        return {
            description: '',
            balance: false,
            notBalance: false,
            specialCharacters: ["(", ")", "{", "}", "[", "]"],
            inputCharacters: [],
            inputUrlEventWaitTimeoutId: 0
        }
    },
    methods: {
        checkPairingParentheses() {
            let index = this.inputCharacters.indexOf('(')
            if (index != -1) {
                if (index < this.inputCharacters.length && this.inputCharacters[index + 1] == ')') {
                    this.checkPairingBracket()
                } else {
                    this.balance = false
                    this.notBalance = true
                }
            } else {
                this.checkPairingBracket()
            }
        },
        checkPairingBracket() {
            let index = this.inputCharacters.indexOf("[")
            if (index != -1) {
                if (index < this.inputCharacters.length - 1 && (this.inputCharacters[index + 1] == "]" || this.inputCharacters[index + 2] == "]")) {
                    this.checkPairingBrace()
                } else {
                    this.balance = false
                    this.notBalance = true
                }
            } else {
                this.checkPairingBrace()
            }
        },
        checkPairingBrace() {
            let index = this.inputCharacters.indexOf("{")
            if (index != -1) {
                if (index < this.inputCharacters.length - 2 && (this.inputCharacters[index + 1] == "}" || this.inputCharacters[index + 2] == "}" || this.inputCharacters[index + 3] == "}")) {
                    this.balance = true
                    this.notBalance = false
                } else {
                    this.balance = false
                    this.notBalance = true
                }
            } else {
                this.notBalance = false
                this.balance = true
            }
        },
        isBalanced() {
            if (this.inputCharacters.length % 2 == 0) {
                this.checkPairingParentheses()
            } else {
                this.balance = false
                this.notBalance = true
            }
        },
        changeDescription() {
            let character = this.description.slice(-1);
            if (character && this.specialCharacters.includes(character)) {
                this.inputCharacters.push(character)
            }
            if (this.inputUrlEventWaitTimeoutId != 0) {
                clearTimeout(this.inputUrlEventWaitTimeoutId);
            }
            this.inputUrlEventWaitTimeoutId = setTimeout(() => {
                this.isBalanced()
            }, 200);
        }
    },
})
</script>

<style scoped></style>