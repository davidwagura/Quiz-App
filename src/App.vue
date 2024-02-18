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
    min-height: 100vh;
  }

  h1 {
    font-size: 2rem;
    margin-bottom: 2rem;
  }

  .quiz {
    background-color: purple;
    padding: 1rem;
    width: 100%;
    max-width: 640px;
    border-radius: 0.5rem;
  }

  .quiz-info {
    display: flex;
    justify-content: space-between;
    margin-bottom: 1rem;
  }

  .quiz-info .question {
    color: gray;
    font-size: 1.25rem;
  }

  .quiz-info .score {
    color: white;
    font-size: 1.25rem;
  }
  .options {
    margin: 1rem;
  }
  .option {
    display: block;
    padding: 1rem;
    background-color: #271C36;
    margin-bottom: 0.5rem;
    border-radius: 0.5rem;
    cursor: pointer;
  }
  .option.hover {
    background-color: #2d213f;
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

    padding: 0.5rem 1rem;
    background-color: green;
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
    color: #afafaf;
    font-size: 1.25rem;
    text-align: center;
  }
</style>
