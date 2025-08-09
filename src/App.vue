<script setup>
import {computed, reactive, ref} from "vue";
import JSConfetti from 'js-confetti'

const jsConfetti = new JSConfetti()

const turns = ref(0);
const canChange = ref(true);

const absoluteMax = 2499999999999
const phone = ref(0);
const bounds = reactive({
  min: 0,
  max: absoluteMax
})

function SetRandom(warmer) {
  if(!canChange.value) {
    return
  }

  if (warmer != null) {
    turns.value++;
    if (warmer) {
      bounds.min = phone.value;
    } else {
      bounds.max = phone.value;
    }
  }
  phone.value = Math.floor(Math.random() * (bounds.max - bounds.min + 1)) + bounds.min;
}

const displayPhone = computed(() => {
  let phoneStr = phone.value.toString();
  while (phoneStr.length < absoluteMax.toString().length) {
    phoneStr = "0" + phoneStr;
  }
  return phoneStr;
})

function RemoveExtraZeroes(str) {
  return Number(str).toString()
}

SetRandom(null)

function Submit(){
  if (!canChange.value) {
    return
  }
  jsConfetti.addConfetti()
  canChange.value = false
}

const copied = ref(false);
function Share(){
  copied.value = true;
  navigator.clipboard.writeText(`I guessed my phone number in ${turns.value} turns on this amazing website: https://xenonppg.github.io/Rofl-UX/`)
}
</script>

<template>
  <div style="display: flex">
    <img src="@/../public/skeleto-skeleton.gif" style="width: 100px"/>
    <h1>Opensource UX phone selector</h1>
  </div>
  <h1>
    +{{ RemoveExtraZeroes(displayPhone.slice(0, 3)) }} ({{ displayPhone.slice(3, 6) }}) {{
      displayPhone.slice(6, 9)
    }}-{{ displayPhone.slice(9, 11) }}-{{ displayPhone.slice(11) }}
  </h1>
  <div style="display: flex;">
    <button @click="() => SetRandom(false)">colder</button>
    <button @click="Submit">submit</button>
    <button @click="() => SetRandom(true)">warmer</button>
  </div>
  <p>Turns: {{ turns }}</p>

  <div style="position: relative">
    <button v-if="!canChange" @click="Share" style="position: absolute">Share</button>
    <p v-if="copied" style="position: absolute; top: 10px">copied</p>
  </div>
</template>

<style>
body {
  height: 90vh;

  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
</style>
