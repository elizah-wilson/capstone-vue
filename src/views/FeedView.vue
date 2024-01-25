<script setup>
import { $ } from 'jquery';
import Header from '../components/Header.vue'
import { ref } from 'vue'

let posts = ref([])

function getPosts() {
    fetch('http://localhost:3000/get-posts',
        {
            headers: { "Authorization": document.cookie },
            method: "GET"
        })
        .then(response => {
            return response.json()
        })
        .then(json => {
            posts.value = { ...json }
            console.log(posts.value)
            /* posts.value = objects with the following keys:
                - date
                -likes
                -page
                - postid
                - prompt
                - response
                - username */
            // used when were getting pages directly from aws s3 using aws sdk:
            // pages.value = [...json].map(key => {return `https://mysvgfiles.s3.us-east-2.amazonaws.com/${key}`})
        })
        .catch(error => {
            console.log
        })
}
getPosts()


function likePost(postId) {

    fetch('http://localhost:3000/like-post', {
        headers: { "Authorization": document.cookie, "Content-Type": "application/json" },
        body: JSON.stringify({ "postId": postId }),
        method: "PUT"
    })
        .then(response => {
            return response.json()
        })
        .then(json => {
            getPosts() // add timestamp column and can order by timestamp
        })
        .catch((error) => {
            console.log(error)
        })
}

</script>

<template>
    <Header id="header"></Header>

    <div id="feed">
        <div v-for="post in posts" id="card">
            <h3> {{ post.username }}</h3>
            <img :src="post.page" id="page">
            <div id="bttm-container">
                <div id="like-container">
                   <button @click="likePost(post.postid)">♡</button>
                    Likes {{ post.likes }}
                </div>
                <div id="caption">
                    <p id="response"> {{ post.response }}</p>
                </div>
            </div>
        </div>

        <!-- <div v-for="page in pages" class="card">
            <img  :src="page" id="page">
        </div> -->
    </div>
</template>

<style scoped>
#feed {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: center;
    gap: 20px;
    margin-bottom: 20px;
}

#page {
    height: 500px;
    width: 500px;
}

#card {
    display: flex;
    flex-direction: column;
    align-items: center;
    background-color: rgb(245, 245, 245);
    color: #0B5D1E;
    height: 100%;
    width: 600px;
    box-shadow: 4px 8px 5px lightgray;
    font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
    font-size: larger;
}

#bttm-container {
    padding: 20px;
}

#likes-container {
    align-self: flex-starts;
}

#caption {
    align-items: center;
    text-align: center;
}

#response {
    height: 100px;
    font-weight: bold;
}

.heart-bttn {
    width: 8em;
    height: 2.5em;
    position: relative;
    font-size: 1.2em;
}

button {
    background: transparent;
    outline: none;
    border: none;
    color: rgba(184, 51, 106, 1);
    letter-spacing: 1px;
    font-size: 1em;
    cursor: pointer;
}

/* creating the 2 halves of the heart bttn */
.heart-bttn button::before, .heart-bttn button::after {
    content: '';
    position: absolute;
    top: 0;
    width: 50%;
    height: 100%;
    background-color: white;
    z-index: 0;
    transition: 0.4s;
}

.heart-bttn button::before {
    left: 0;
    border-radius: 2em 0 0 2em;
}

.heart-bttn button::after {
    right: 0;
    border-radius: 0 2em 2em 0;
}

/* positioning text & textbox */
.heart-bttn span {
    position: absolute;
    z-index: 1;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}

input[type="checkbox"] {
    position: absolute;
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
    opacity: 0;
    z-index: 2;
}

/* rotate the halves & hide the text when the checkbox is pressed + animation*/
input[type="checkbox"]:checked + button::before {
    transform: rotate(45deg) translate(1em, -1.05em);
    animation: glow 0.6s ease-in 0.4s forwards;
}

input[type="checkbox"]:checked + button::after {
    transform: rotate(-45deg) translate(-1rem, -1.05em);
    animation: glow 0.6s ease-in 0.4s forwards;
}

input[type="checkbox"]:checked + button span {
    opacity: 0;

} 

@keyframes glow {
    100% {
        background: rgba(184, 51, 106, 1);
    }
}

</style>