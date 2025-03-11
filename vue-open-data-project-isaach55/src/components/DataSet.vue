<template>
  <div></div>
</template>

<script setup>
import { ref } from 'vue'

const apiURL = `https://data.cityofnewyork.us/resource/43nn-pn8j.json`

//create a new url by filtering
//https://dev.socrata.com/docs/queries/ documentation !

async function getData() {
  const dataURL = `https://data.cityofnewyork.us/resource/43nn-pn8j.json?cuisine_description=Italian&$limit=50000`
  let res = await fetch(dataURL)
  let data = await res.json()
  console.log(data)
  //samantha rat core
  //d3.js for maps?
  //"autofilling" search bar
}

getData()
</script>

<style scoped></style>
