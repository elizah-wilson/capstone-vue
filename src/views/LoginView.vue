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
            headers: { "Content-Type": "application/json" },
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
    <div class="flex-container">
        <div class="flex-column">
            <label for="email">Email:</label>
            <input type="email" id="email" name="login" v-model="email" class="input-size">
            <label for="password">Password:</label>
            <input type="password" id="password" name="login" v-model="password" class="input-size">
            <button id="login-bttn" @click="login()" class="input-size">Login</button>
            <p>
                Not a member yet? <RouterLink to="/register" id="link">Register!</RouterLink>
            </p>
        </div>

        <div id="about">
            <h1>The Coloring Book Club</h1>
            <h4 id="about-text">
                An exclusive online community for creativity, reflection, and positivity - 
                <br>
                where each day brings a new coloring page and writing prompt, designed to promote relaxation and mindfulness.
                <br>
                Showcase your creations and thoughts on our feed wall
                <br>
                to inspire and be inspired.
                <br>
                Join us in the art of mindful living.
            </h4>
        </div>
    </div>

</template>

<style scoped>
* {
    font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;

}

#link {
    text-decoration: none;
    color: rgba(184, 51, 106, 1);
    font-weight: bolder;
}

.flex-container {
    display: flex;
    justify-content: space-evenly;
    align-items: center;
    width: 100%;
    height: 100vh;
}

.flex-column {
    display: flex;
    flex-direction: column;
    justify-content: center;
    gap: 20px;
}

#about {
    width: 800px;
    height: 600px;
    background: rgb(208,241,191);
    background: radial-gradient(circle, rgba(208,241,191,1) 0%, rgba(182,215,185,1) 100%);
    color: rgba(184, 51, 106, 1);
    border-radius: 40% 60% 60% 40% / 70% 30% 70% 30%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    text-align: center;
}

#about-text {
    width: 600px;
}

#login-bttn {
    background-color: rgba(184, 51, 106, 1);
    border: rgba(184, 51, 106, 1);
    border-radius: 5px;
    color: white;
    font-weight: bolder;
    font-size: large;
    cursor: pointer;
}

.input-size {
    font-size: large;
    padding: 10px;
}


</style>