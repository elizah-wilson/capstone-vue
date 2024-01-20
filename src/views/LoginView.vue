<script setup>
import { ref } from 'vue'
import router from '@/router';
import { RouterLink } from 'vue-router'

let email = ref('')
let password = ref('')

function login() {

    const reqBody = {
        "email": email.value,
        "password": password.value
    }

    fetch("http://localhost:3000/login",
        {
            headers: { "Content-Type":"application/json" },
            body: JSON.stringify(reqBody),
            method: "POST"
        })
        .then(response => {
            return response.json()
        })
        .then(json => {
            document.cookie = json
            router.push('/')
        })
        .catch(error => {
            console.log(error)
            alert('Email and/or Password incorrect. Try again.')
        })
}
</script>

<template>
    <label for="email">Email:</label>
    <input type="email" id="email" name="login" v-model="email">
    <label for="password">Password:</label>
    <input type="password" id="password" name="login" v-model="password">
    <button id="login-bttn" @click="login()">Login</button>
    <p>
        Not a member yet? <RouterLink to="/register" id="link">Register!</RouterLink>
    </p>
</template>

<style scoped>
#link {
    text-decoration: none;
    color: rgba(184, 51, 106, 1);
    font-weight: bolder;
}

.flex-container {
    display: flex;
}

.flex-column {
    display: flex;
    flex-direction: column;
}
</style>