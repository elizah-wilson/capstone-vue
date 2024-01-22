<script setup>
import { RouterLink } from 'vue-router'
import { ref } from 'vue'


function currentDate() {
    const current = new Date();
    const date = `${current.getMonth() + 1}/${current.getDate()}/${current.getFullYear()}`;
    return date;
}

// 24-hour Countdown Timer
setInterval(function time() {
    var d = new Date();
    var hours = 24 - d.getHours();
    var min = 60 - d.getMinutes();
    if ((min + '').length == 1) {
        min = '0' + min;
    }
    var sec = 60 - d.getSeconds();
    if ((sec + '').length == 1) {
        sec = '0' + sec;
    }
    jQuery('#countdown h2').html(hours + ':' + min + ':' + sec).addClass()
}, 1000);

</script>

<template>
    <div id="header">
        <h2 id="date"> {{ currentDate() }}</h2>
        <h1 id="title">The Coloring Book Club</h1>
        <div id="links-container">
            <RouterLink to="/">Home</RouterLink>
            <RouterLink to="/feed">Feed</RouterLink>
            <RouterLink to="/account">My account</RouterLink>

        </div>
        <div id="countdown">
            <h2></h2>
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

#date {
    margin-left: 3rem;
    color: rgba(184, 51, 106, 1);
}

#title {
    color: white;
}

#countdown {
    margin-right: 3rem;
    color: rgba(184, 51, 106, 1);
}
</style>