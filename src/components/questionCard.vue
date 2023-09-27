<script setup>

import { ref } from 'vue';

const urlApi = 'https://opentdb.com/api.php?amount=1&difficulty=easy&type=multiple'
const tarjeta = ref([])
const acierto = ref('false');
const array = ref(['', '', '', '']);
const contestada = ref(false);
const pregunta = ref('')
const categoria = ref('/src/assets/img/');
const correctAnswer = ref('');
const contadorAcierto = ref(0);
const contadorError = ref(0);

const fetchQuestion = () => {
    fetch(urlApi)
      .then( ( respuesta ) =>  respuesta.json()  )
      .then( ( data ) => { 
        const q = data.results[0]
        q.incorrect_answers.push(q.correct_answer)
        array.value = q.incorrect_answers
        pregunta.value = q.question
        categoria.value = q.category.split(' ')[0].replace(':', '').toLowerCase()
        correctAnswer.value = q.correct_answer
        shuffledArr()
      
    } )

}; 


fetchQuestion()

  
  const salidaClase = () => {
    return {
      "link-error": contestada.value && !acierto.value,
      "link-success": contestada.value && acierto.value,
      "link link-hover": !contestada.value,
    };
  };
  const shuffledArr = () => {
    contestada.value = false;
    for (let i = array.value.length - 1; i > 0; i--) {
      const j = Math.floor(Math.random() * (i + 1));
      [array.value[i], array.value[j]] = [array.value[j], array.value[i]];
    }
  };

  const respuestaCorrecta = (respuesta) => {
    console.log(respuesta, respuesta === correctAnswer.value);
    acierto.value = respuesta === correctAnswer.value;
    if (acierto.value == true) {
      contadorAcierto.value++
      
    } else {
      contadorError.value++
    }
    contestada.value = true;
    setTimeout(function() {
      fetchQuestion()}, 2000);
    
    
    
    

  };
  
  

</script>

<template>



<div class="m-10 card lg:card-side bg-base-100 shadow-xl lg:w-5/6 m-auto sm:w-9/12">
    <figure class="h-auto m-2"><img :src="'/src/assets/img/' + categoria + '.jpg'" :alt="categoria"/></figure>
    <div class="card-body sm:w-full md:w-full xl:w-3/12 ">
      <h2 class="card-title text-2xl " v-html="pregunta"></h2>
      <ul class="ms-5 list-disc mb-6">
        <li class="text-lg link link-hover text-2xl mt-2 mb-6"
        v-for="respuesta in array"
        :key="respuesta"
        @click="respuestaCorrecta(respuesta)"
        >
        <div :class="salidaClase()" v-html="respuesta"></div>
        </li>
    </ul>
      <div class="card-actions flex absolute bottom-4 right-4">
        <button class="btn btn-secondary lg:text-2xl" @click="shuffledArr">Shuffle Answers</button>
        <button class="btn btn-primary lg:text-2xl" @click="fetchQuestion">Skip Question</button>
        
      </div>
    </div>
  </div>

  <div class="m-10 mt-16 lg:w-5/6 m-auto sm:w-9/12">
        <p class="float-left bg-green-500 p-4 rounded-lg text-white border-4 border-white">Right Answers: <span v-html="contadorAcierto"></span></p>
        <p class="float-right bg-orange-600 p-4 rounded-lg text-white border-4 border-white">Wrong Answers: <span v-html="contadorError"></span></p>
  </div>

</template>