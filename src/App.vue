<script setup>

  import { ref, computed } from 'vue'

  const questions = ref([
    {
      question: 'what is vue JS?',
      answer: 0,
      options: [
        'A front end framework',
        'A library',
        'An ice cream maker'
      ],
      selected: null
    },
    {
      question: 'what is vueX?',
      answer: 2,
      options: [
        'Vue with an x',
        'A cheese selection',
        'State management library'
      ],
      selected: null
    },
    {
      question: 'what is Vue Router used for?',
      answer: 1,
      options: [
        'A routing libraly for vue JS',
        'Walking in space',
        'Burger sauce',
        'Quizes'
      ],
      selected: null
    },


  ])

  const quizCompleted = ref (false)
  const currentQuestion =  ref(0)
  const score = computed(() => {
    let value = 0
    questions.value.map(q => {
      if (q.selected == q.answer) {
        value++
      }
    })
    return value
  })
  
  const getCurrentQuestion = computed (() => {
    let question = questions.value[currentQuestion.value]
    question.index = currentQuestion.value
    return question
  })
  
  const SetAnswer = evt => {
    questions.value[currentQuestion.value].selected = evt.target.value
    evt.target.value = null
  }

const NextQuestion = () => {
  if(currentQuestion.value < questions.value.length - 1) {
    currentQuestion.value++
  }
  else {
    quizCompleted.value = true
  }
}

</script>

<template>
  <main class="app">
    <h1>The Quiz</h1>

    <section class="quiz" v-if="!quizCompleted">
      <div class="quiz-info">
        <span class="question">
          {{ getCurrentQuestion.question }}
        </span>
        <span class="store">Store {{ score }} / {{ questions.length }}</span>  
      </div>

      <div class="options">
        <label for="(option, index) in getCurrentQuestion.options"
         :key="index" 
         :class="`option ${
          getCurrentQuestion.selected == index
            ? index == getCurrentQuestion.answer
              ? 'correct'
              : 'wrong'
            : ''
          
         } ${
            getCurrentQuestion.selected != null &&
            index !=getCurrentQuestion.selected
              ? 'disabled'
              : ''

         }`">


          <input type="radio" 
            name="getCurrentQuestion.index"
            value="index"
            v-model="getCurrentQuestion.selected"
            :disabled="getCurrentQuestion.selected"
            @change="SetAnswer">
            <span>{{ options }}</span>
        </label>
      </div>

      <button
      @click="NextQuestion"
      :disabled="!getCurrentQuestion.selected">
      {{ 
        getCurrentQuestion.index == question.length - 1
          ? 'Finish'
          :getCurrentQuestion.selected == null
          ? 'select an option'
          : 'Next Question'
      }}

      </button>

    </section>
    <section v-else>
      <h2>You have finished the quiz</h2>
      <p>Your score is {{ score }} / {{ question.length }}</p>
    </section>
  </main>
</template>

<style scoped>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'montserat', sans-serif;
  }
  body {
    background-color: rgb(50, 1, 50);
    color: #FFF;
  }
  .app {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 2rem;
    
  }
</style>
