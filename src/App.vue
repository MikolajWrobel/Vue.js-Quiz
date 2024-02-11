<script setup>

import {ref, computed} from 'vue'

const questions = ref([
  {
      question: 'Co jest stolicą Polski?',
      answer: 0,
      options: [
        'Warszawa',
        'Kraków',
        'Gniezno'
      ],
      selected: null
  },
  {
      question: 'Ile jest województw w Polsce?',
      answer: 2,
      options: [
        '14',
        '15',
        '16'
      ],
      selected: null
  },
  {
      question: 'W którym województwie znajduje się Olsztyn?',
      answer: 2,
      options: [
        'Mazowieckim',
        'Małopolskim',
        'Warmińsko-Mazurskim',
        'Pomorskim'
      ],
      selected: null
  }
])

const quizCompleted = ref(false)
const currentQuestion = ref(0)
const score = computed(() => {
  let value = 0
  questions.value.map(q => {
    if (q.selected == q.answer) {
      value++
    }
  })
  return value
})

const getCurrentQuestion = computed(() => {
  let question = questions.value[currentQuestion.value]
  question.index = currentQuestion.value
  return question
})

const SetAnswer = evt => {
  questions.value[currentQuestion.value].selected = evt.target.value
  evt.target.value = null
}
 
const NextQuestion = () => {
  if (currentQuestion.value < questions.value.length - 1) {
    currentQuestion.value++
  } else {
    quizCompleted.value = true
  }
  }

</script>

<template>

  <main class="app">
    <h1> Quiz </h1>

    <section class="quiz" v-if="!quizCompleted">
      <div class="quiz-info">
        <span class="question">{{ getCurrentQuestion.question }}</span>
        <span class="score"> Score {{ score }} / {{ questions.length }}</span>
      </div>

      <div class="options">
        <label v-for="(option, index) in getCurrentQuestion.options" 
            :key="index"
            :class="`option ${
              getCurrentQuestion.selected == index
                ? index == getCurrentQuestion.answer 
                  ? 'correct'
                  : 'wrong'
                :''
            } ${
              getCurrentQuestion.selected != null &&
              index != getCurrentQuestion.selected
                  ? 'disabled'
                  : ''
            }`">
          <input 
              type="radio" 
              :name="getCurrentQuestion.index"
              :value="index"
              v-model="getCurrentQuestion.selected"
              :disabled="getCurrentQuestion.selected"
              @change="SetAnswer">
          <span>{{ option }}</span>
        </label>
      </div>

      <button
        @click="NextQuestion"
        :disabled="!getCurrentQuestion.selected">
        {{ 
          getCurrentQuestion.index == questions.length - 1
            ? "Zakończ Quiz"
            : getCurrentQuestion.selected == null
              ? 'Wybierz Odpowiedź'
              : 'Następne Pytanie'
        
        }}
      
      
      </button>
    </section>
    <section v-else>
      <h2>Brawo! Ukończyłeś Quiz!</h2>
      <p> Twój wynik to: {{ score }} / {{ questions.length }}</p>
    </section>


  </main>
</template>

<style>

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: sans-serif;
}

body {
  background-color: #516eff;
  color: white
}

.app {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 2rem;
  min-height: 100vh;
}

h1 {
  font-size: 2rem;
  margin-bottom: 2rem;
}


.quiz {
  background-color: #43bdbd;
  padding: 1rem;
  width: 640px  ;
  
  border-radius: 0.5rem;
}

.quiz-info {
  display: flex;
  justify-content: space-between;
  margin-bottom: 1rem;
}

.quiz-info .question {
  color: rgb(0, 0, 0);
  font-size: 1.25rem;
  font-weight: 100;
}

.quiz-info .score {
  color: white;
  font-size: 1.25rem;
}

.options {
  margin-bottom:  1rem;
}

.option {
  display: block;
  padding: 1rem;
  background-color: rgb(0, 179, 255);
  margin-bottom: 0.5rem;
  border-radius: 0.5rem;
  cursor: pointer;
}

.option:hover {
  background-color: rgb(107, 213, 255);
}

.option.correct {
  background-color: #2cce7d;
}

.option.wrong {
  background-color: #ff5a5f;
}

.option:last-of-type {
  margin-bottom: 0;
}

.option.disabled {
  opacity: 0.5;
}

.option input {
  display: none;
}

button {
  appearance: none;
  outline: none;
  border: none;
  cursor: pointer;
  
  padding: 0.5rem 1 rem;
  background-color: #2cce7d;
  color: #2d213f;
  font-weight: 700;
  text-transform: uppercase;
  font-size: 1.25rem;
  border-radius: 0.5rem;
}

button:disabled {
  opacity: 0.5;
}

h2 {
  font-size: 2rem;
  margin-bottom: 2rem;
  text-align: center;
}

p {
  color: #000000;
  font-size: 1.25rem;
}


</style>
