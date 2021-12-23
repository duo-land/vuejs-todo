<template lang="pug">
.app-wrapper
  TodoApp
</template>

<script setup>
import TodoApp from './components/TodoApp.vue'
</script>

<style lang="stylus">
html, body
  padding: 0;
  margin: 0;
  background: #fff;

h1
  margin: 0;

.app-wrapper
  padding 10px
#app
  font-family Avenir, Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  text-align center
  color #2c3e50

</style>
