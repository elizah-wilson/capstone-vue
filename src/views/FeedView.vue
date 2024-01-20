<script setup>
import Header from '../components/Header.vue'
import { ref } from 'vue'

let pages = ref([])


fetch('http://localhost:3000/get-objects',
    {
        headers: { "Authorization": document.cookie },
        method: "GET"
    })
    .then(response => {
        return response.json()
    })
    .then(json => {
        
        // good practice to assign variables with value of array to a value with spreader
        pages.value = [...json].map(key => {
            return `https://mysvgfiles.s3.us-east-2.amazonaws.com/${key}`
        })
        console.log(pages.value)
    })
    .catch(error => {
        console.log
    })


</script>

<template>
    <Header id="header"></Header>

    <div id="feed">

        <img v-for="page in pages" :src="page">

    </div>
</template>

<style scoped>
    img {
        height: 50px;
        width: 50px;
    }
</style>