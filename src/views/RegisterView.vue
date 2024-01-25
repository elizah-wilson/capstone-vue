<script setup>
import { RouterLink } from 'vue-router';

function register() {
    
}

</script>

<template>

    <div class="flex-container">
        <div class="flex-column">
            <label for="username">Username:</label>
            <input type="text" id="username" name="registration" class="input-size">
            <label for="email">Email:</label>
            <input type="email" id="email" name="login" v-model="email" class="input-size">
            <label for="password">Password:</label>
            <input type="password" id="password" name="login" v-model="password" class="input-size">
            <button id="register-bttn" @click="" class="input-size">Join the Club!</button>
          
        </div>

        <div id="about">
            <h1>The Coloring Book Club</h1>
            <p id="about-text">
                Exclusive online community that uses coloring and writing prompts to reflect on life, and promote relaxation, mindfulness and positivity.
                <br>
                Each day there is a new coloring page and mindful writing prompt that you can post to the feed wall to showcase your creativity and thoughts to the club. 
            </p>
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
    font-weight: bold;
}

#register-bttn {
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