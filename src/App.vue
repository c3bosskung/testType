<script setup>

import {ref} from 'vue'
import {wordArr} from "@/data/wordArr";

let letter = ref([])
let countIndex = 0

let score = ref(0)

let history = ref({})

let word = []

for (let i = 0; i < 20; i++) {
  let ranWord = wordArr[Math.floor(Math.random() * wordArr.length)]
  if(word.every(e => e !== ranWord)) {
    word.push(ranWord)
  } else {
    i--
  }
  if(word.length > 20) {
    break
  }
}
word = word.join(' ')
word.split('').forEach((e, i) => history.value[i] = false)
let spiltWord = word.split(' ')

const isCorrect = (word, letter, index) => {
  if (word[index] === letter[index] &&  (Object.keys(history.value)[index] === '0' || history.value[index - 1])) {
    history.value[index] = true
    return true
  }
  return false
}

const increaseScore = () => {
  score.value <= spiltWord.length - 1? score.value++ : score.value
}

window.addEventListener('keydown', (event) => {
  // console.log(event.key)
  if (event.key.length <= 1 && countIndex < Object.keys(history.value).length) {
    letter.value.push(event.key)
    countIndex++
  } else if (event.key === 'Backspace' && !isCorrect(word, letter.value, countIndex - 1)) {
    letter.value.pop()
    history.value[countIndex <= 0? 0 : countIndex - 1] = false
    countIndex <= 0? countIndex = 0 : countIndex--
  }

  if(countIndex === Object.keys(history.value).length && isCorrect(word, letter.value, countIndex - 1)) {
    increaseScore()
  }

  if(event.key === ' ' && isCorrect(word, letter.value, countIndex - 1)) {
    increaseScore()
  } else if (event.key === 'Backspace' && word[countIndex] === ' ' && history.value[countIndex - 1] && !isCorrect(word, letter.value, countIndex - 1)) {
    score.value <= 0? score.value = 0 : score.value--
  }

  //console.log(isCorrect(word, letter.value, countIndex - 1))
  //console.log(word.at(countIndex - 1) + ' ' + letter.value[countIndex - 1])
  // console.log(countIndex)
  // console.log(word.split(' ').length)
})


</script>

<template>
  <div class="w-full p-10 bg-orange-200">
    <div class="w-full m-5 ">
      <h2> <b>score: </b> {{ score }}/{{ spiltWord.length }} {{ score === spiltWord.length? 'finish' : '' }}</h2>
      <span v-for="(item,index) in word" :key="index"
            :class=" isCorrect(word, letter, index)? 'bg-green-500' : index > countIndex - 1? 'bg-gray-500' : 'bg-red-500' ">{{
          item
        }}</span>
      <p>{{ letter.join('') }}</p>
      <br>
      <p> history:  {{ history }}</p>
      <br>
      <p> history value {{ Object.keys(history)[countIndex] }}</p>
      <br>
      <p> {{ spiltWord }}</p>
      <br>
      <p> countIndex: {{ countIndex }}</p>
      <br>
    </div>
  </div>
</template>

<style scoped>

</style>
