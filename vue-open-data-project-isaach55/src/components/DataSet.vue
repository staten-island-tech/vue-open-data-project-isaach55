<template>
  <div></div>
</template>

<script setup>
import { ref } from 'vue'

const restaurantData = ref('')
async function getData() {
  let res = await fetch(https://data.cityofnewyork.us/resource/43nn-pn8j.json)
  let data = await res.json
  //samantha rat core
  //d3.js for maps?
}
</script>

<style scoped></style>
