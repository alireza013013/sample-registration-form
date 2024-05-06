<template>
    <div class="flex items-center justify-center flex-wrap w-full max-w-xs rounded-xl h-64">
        <header class="w-full h-16 rounded-t-xl flex justify-between items-center px-2.5">
            <h1 class="text-xl font-bold text-black">{{ steps[currentStep - 1] }}</h1>
            <span class="text-xl font-medium text-black">{{ currentStep }} / 3</span>
        </header>
        <div class="flex items-center justify-evenly flex-col w-full rounded-b-xl bg-white h-52">
            <div class="flex flex-col items-start justify-start w-11/12 gap-y-2.5" v-if="currentStep == 1">
                <label class="w-full font-semibold text-base">Username:</label>
                <input v-model="userName" type="text" id="username" name="username" placeholder="UserName">
                <span class="text-xs font-normal text-red-600" v-if="errorUserName">{{ errorUserName }}</span>
            </div>


            <div class="flex flex-col items-start justify-start w-11/12 gap-y-2.5" v-if="currentStep == 2">
                <label class="w-full font-semibold text-base">Email:</label>
                <input v-model="email" id="email" name="email" type="text" placeholder="Email">
                <span class="text-xs font-normal text-red-600" v-if="errorEmail">{{ errorEmail }}</span>
            </div>

            <div class="w-11/12 flex flex-col" v-if="currentStep == 3">
                <span class="text-base text-black font-semibold">
                    Username: {{ userName }}
                </span>
                <span class="text-base text-black font-semibold">
                    Email: {{ email }}
                </span>
            </div>


            <div class="flex gap-x-2.5">
                <button id="btn-prev" :disabled="currentStep == 1" :onclick="previousStep">previous</button>
                <button id="btn-next" :disabled="currentStep == 3" :onclick="nextStep">next</button>
            </div>
        </div>
    </div>

</template>


<script>
import { defineComponent } from 'vue'

export default defineComponent({
    data() {
        return {
            userName: undefined,
            errorUserName: undefined,
            email: undefined,
            errorEmail: undefined,
            currentStep: 1,
            steps: ["Step: username", "Step: email", "Step: review"]
        }
    },
    methods: {

        previousStep() {
            if (this.currentStep > 1) {
                this.currentStep = this.currentStep - 1
            }
        },
        nextStep() {
            if (this.currentStep < 3) {
                let validationUserName = false;
                let validationEmail = false;
                if (this.currentStep == 1) {
                    validationUserName = this.validateUserName()
                }
                if (this.currentStep == 2) {
                    validationEmail = this.validateEmail()
                }
                if (validationUserName || validationEmail) {
                    this.errorUserName = ""
                    this.errorEmail = ""
                    this.currentStep = this.currentStep + 1
                }
            }
        },
        validateEmail() {
            const regularExpression = new RegExp(
                /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
            );
            if (!regularExpression.test(this.email)) {
                this.errorEmail = "Invalid email address."
                return false
            } else if (this.email == "" || this.email == undefined) {
                this.errorEmail = "Invalid email address. Username must not be empty"
                return false
            }
            return true
        },
        validateUserName() {
            if (this.userName?.length < 4) {
                this.errorUserName = "Invalid Username. Username must be more than four characters"
                return false
            } else if (this.userName?.length > 20) {
                this.errorUserName = "Invalid Username. Username must be less than twenty characters"
                return false
            } else if (this.userName?.includes("@")) {
                this.errorUserName = "Invalid Username. Username should not contain @"
                return false
            } else if (this.userName == "" || this.userName == undefined) {
                this.errorUserName = "Invalid Username. Username must not be empty"
                return false
            }
            return true
        }
    }

})


</script>



<style scoped>
header {
    background-color: #94affd;
}

input {
    width: 100%;
    background-color: transparent;
    border: 1px solid black;
    border-radius: 8px;
    font-weight: 500;
    font-size: 14px;
    color: black;
    padding: 8px 12px;
}

input:focus {
    outline: none !important;
    border: 1px solid black;
}

input::placeholder {
    font-weight: 500;
    font-size: 14px;
    color: rgb(88, 88, 88);
}

input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
    -webkit-appearance: none;
    margin: 0;
}


button {
    width: 80px;
    height: 30px;
    background-color: #94affd;
    border-radius: 6px;
    color: black;
    font-size: 14px;
    font-weight: 600;
    border: none;
    cursor: pointer;
}

button:active {
    transform: scale(1.1);
}

button:disabled {
    opacity: 0.5;
    cursor: not-allowed;

    &:active {
        transform: scale(1);
        cursor: not-allowed;
    }
}
</style>