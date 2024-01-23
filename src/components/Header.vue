<script setup>
import { RouterLink } from 'vue-router'
import { ref } from 'vue'


// 24-hour Countdown Timer - removed not necessary
// setInterval(function time() {
//     var d = new Date();
//     var hours = 24 - d.getHours();
//     var min = 60 - d.getMinutes();
//     if ((min + '').length == 1) {
//         min = '0' + min;
//     }
//     var sec = 60 - d.getSeconds();
//     if ((sec + '').length == 1) {
//         sec = '0' + sec;
//     }
//     jQuery('#countdown h2').html(hours + ':' + min + ':' + sec).addClass()
// }, 1000);

</script>

<template>
    <div id="header">
        <h1><RouterLink to="/" class="links logo">The Coloring Book Club</RouterLink></h1>
        <div id="links-container">
            <RouterLink to="/feed" class="links">Feed</RouterLink>
            <RouterLink to="/account" class="links">My account</RouterLink>

        </div>
        
    </div>
</template>


<style scoped>
#header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    height: 100px;
    width: 100%;
    background: rgb(182, 215, 185);
    background: linear-gradient(90deg, rgba(182, 215, 185, 1) 0%, rgba(184, 51, 106, 1) 50%, rgba(208, 241, 191, 1) 100%);
    margin-bottom: 10px;
    font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
}

.logo {
    margin-left: 5rem;
}

.links {
    text-decoration: none;
    color: white;
}

#links-container {
    display: flex;
    font-size: larger;
    font-weight: bold;
    gap: 20px;
    margin-right: 5rem;
}


</style>