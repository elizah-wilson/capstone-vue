<script setup>
import { RouterLink } from 'vue-router'

</script>

<template>

    <div id="header">
        <h1 id="title">The Coloring Book Club</h1>
    </div>
</template>


<style scoped>
#header {
    height: 150px;
    width: 100%;
    background: rgb(182, 215, 185);
    background: linear-gradient(90deg, rgba(182, 215, 185, 1) 0%, rgba(184, 51, 106, 1) 50%, rgba(208, 241, 191, 1) 100%);
    margin-bottom: 20px;

#title {
  font-family:'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
  color: white;
  text-align: center;
}

}
</style>