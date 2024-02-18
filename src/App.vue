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
  <h1>Hello world</h1>
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
</style>
