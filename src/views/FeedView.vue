<script setup>
import Header from '../components/Header.vue'
import { ref } from 'vue'

let posts = ref([])


fetch('http://localhost:3000/get-posts',
    {
        headers: { "Authorization": document.cookie },
        method: "GET"
    })
    .then(response => {
        return response.json()
    })
    .then(json => {
        
        posts.value = {...json}
        console.log(posts.value)
        // pages.value = [...json].map(key => {return `https://mysvgfiles.s3.us-east-2.amazonaws.com/${key}`})
    })
    .catch(error => {
        console.log
    })


</script>

<template>
    <Header id="header"></Header>

    <div id="feed">
        <div v-for="post in posts" class="card">
            <p> {{ post.username }}</p>
            <img :src="post.page" id="page">
            <div>
                <button @click="">Like</button>
                Likes {{ post.likes }}
            </div>
            <p> "{{ post.prompt }}"</p>
            <p> {{ post.response }}</p>
        </div>

        <!-- <div v-for="page in pages" class="card">
            <img  :src="page" id="page">
        </div> -->
    </div>
</template>

<style scoped>
    #feed {
        display:flex;
        flex-direction: column;
        gap: 20px;
        margin-bottom: 20px;
    }

    #page {
        height: 500px;
        width: 500px;
    }

    .card {
        display: flex;
        flex-direction: column;
        align-items: center;
        background-color: whitesmoke;
        color: #0B5D1E; 
        height: 100%;
        width: 600px;
        box-shadow: 4px 8px 5px lightgray;

    }

</style>