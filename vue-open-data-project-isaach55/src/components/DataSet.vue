<template>
  <div></div>
</template>

<script setup>
import { ref } from 'vue'

const apiURL = `https://data.cityofnewyork.us/resource/43nn-pn8j.json`
const query = `?$where=UPPER(cuisine_description) LIKE '%25Chinese%25' &$where=UPPER(violation_description) LIKE '%25RATS%25' OR UPPER(violation_description) LIKE '%25MICE%25'&$limit=50000`

//create a new url by filtering
//https://dev.socrata.com/docs/queries/ documentation !

async function getData() {
  const dataURL = `${apiURL}${query}`
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
