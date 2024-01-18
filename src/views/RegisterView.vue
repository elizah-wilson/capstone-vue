<script setup>
import Header from '../components/Header.vue'
</script>

<template>
    <label for="username">Username:</label>
    <input type="text" id="username" name="registration">

    <label for="email">Email:</label>
    <input type="email" id="email" name="registration">

    <label for="password">Password:</label>
    <input type="password" id="password" name="registration">

    <button>Register</button>
</template>

<style scoped></style>