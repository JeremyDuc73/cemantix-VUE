<script setup>
import {ref, watchEffect, reactive} from "vue";
const word = ref("")
const inputWord = ref("")
const compareValue = ref(null)
const wordsUsed = reactive([])
const values = reactive([])

watchEffect(async () => {
  const url = `https://127.0.0.1:8000/fetch/${word.value}`
  compareValue.value = await (await fetch(url)).json()
  if (!values.includes(compareValue.value))
  {
    values.push(compareValue.value)
  }
})

function sendWord(){
  word.value = inputWord.value
  if (!wordsUsed.includes(word.value))
  {
    wordsUsed.push(word.value)
  }

}
</script>


<template>
  <input v-model.lazy="inputWord">
  <button @click="sendWord">Send</button>
  <p v-if="!compareValue">Loading...</p>
  <pre v-else>{{compareValue}}</pre>

  <div v-for="w in wordsUsed" v-bind:key="w.value">
    <p>{{w}}</p>
  </div>
  <div v-for="v in values" v-bind:key="v.value">
    <p>{{v}}</p>
  </div>
</template>


<style>

</style>
