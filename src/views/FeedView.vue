<script setup>
import Header from '../components/Header.vue'
import { ref } from 'vue'

let pages = ref([])


fetch('http://localhost:3000/get-objects')
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

    <div v-for="page in pages" id="feed">
        <!-- escape slash  using literal interopolation-->
        <img :src="page" >
        <!-- use a filter function that will display pages that match the current date :) -->
    </div>
    
</template>

<style scoped>

</style>