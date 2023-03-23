

<template>
    <div class="page">
        <div class="modal">
            <h1 class="modal-title">Email verification</h1>
            <p class="modal-subtitle">Let’s get started with this simple verification.</p>
            <form @submit.prevent="submitForm" class="form" ref="form">
                {{ url }}
                <div class="input_group">
                    <label for="email" class="input_group-label">Email verification</label>
                    <input :class="[{'m-error':emailErrorText.length}, 'input_group-field']" type="email" id="email" placeholder="example@mail.com"
                        v-model="email" name="email" @blur="validateEmail">
                    <div class="input_group-error" v-if="emailErrorText.length">
                        <img class="input_group-error_icon" src="../assets/images/icon-error.svg" />
                        {{ emailErrorText }}
                    </div>
                </div>
                <div class="input_group">
                    <label for="url" class="input_group-label">Verification URL</label>
                    <input :class="[{'m-error':urlErrorText.length}, 'input_group-field']" type="text" id="url" placeholder="https://"
                        v-model="url" name="url" @blur="validateUrl">
                    <div class="input_group-error" v-if="urlErrorText.length">
                        <img class="input_group-error_icon" src="../assets/images/icon-error.svg" />
                        {{ urlErrorText }}
                    </div>
                </div>
                <div class="input_group m-last">
                    <button type="submit" @click.prevent="submitForm" class="input_group-button" :disabled="!isFormValid">Move forward</button>
                    <div class="input_group-error" v-if="formErrorText.length">
                        <img class="input_group-error_icon" src="../assets/images/icon-error.svg" />
                        {{ formErrorText }}
                    </div>
                    
                </div>
            </form>
        </div>
    </div>
</template>

<script setup>
    import { ref, computed } from 'vue'

    const form = ref('');
    const email = ref('');
    const url = ref('');
    const emailValid = ref(false);
    const urlValid = ref(false);
    const emailErrorText = ref('');
    const urlErrorText = ref('');
    const formErrorText = ref('');

    const validateEmail = () => {
        if (email.value.length) {
            emailValid.value = /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email.value)
            emailErrorText.value = emailValid.value ? "" : "Looks like it’s not an email. Check it."
        } else {
            emailValid.value = false
            emailErrorText.value = "Looks like you forgot to put in email."
        }
    }

    const validateUrl = () => {
        if (url.value.length) {
            urlValid.value = /^https:\/\/[a-zA-Z0-9]+\.([a-zA-Z].*)$/.test(url.value)
            urlErrorText.value = urlValid.value ? "" : "What about security bro?"
        } else {
            urlValid.value = false
            urlErrorText.value = "This field is required and cannot be left empty"
        }
    }

    const isFormValid = computed(() => emailValid.value && urlValid.value);

    const submitForm = async () => {
        if (isFormValid.value) {
            try {
                await fetch(url.value, {
                    method: "POST",
                    body: new FormData(form.value)
                })
            } catch(err) { 
                console.error(err.message);
            }

            window.location.href = "https://payproglobal.com/";
        } else {
            formErrorText.value = "Try to fill all fields correctly"
        }
    }

</script>

<style scoped lang="scss">
    @import './home';
</style>
