<script setup>
  import { ref } from 'vue';
  import questionCard from './components/questionCard.vue'
  
  

  
</script>

<template>
  
 <h1 class="text-7xl m-5 ms-8 font-bold text-white text-center">Let's Play Trivia!</h1>

  
<questionCard/>
<score/>
  

</template>



<style>

body {
    background-image: url('./assets/img/bg.jpg');
    background-size: cover;
  }

</style>