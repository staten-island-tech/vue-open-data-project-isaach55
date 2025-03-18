<template>
  <div></div>
</template>

<script setup>
import { ref } from 'vue'

const apiURL = `https://data.cityofnewyork.us/resource/43nn-pn8j.json`
const query = `?$where=UPPER(cuisine_description) LIKE '%25CHINESE%25' AND (UPPER(violation_description) LIKE '%25RATS%25' OR UPPER(violation_description) LIKE '%25MICE%25')&$limit=50000`

//`https://data.cityofnewyork.us/resource/43nn-pn8j.json?$select=distinct cuisine_description`

//create a new url by filtering
//https://dev.socrata.com/docs/queries/ documentation !
//https://data.cityofnewyork.us/resource/43nn-pn8j.json?$where=UPPER(cuisine_description)%20LIKE%20%27%25CHIMICHURRI%25%27&$limit=50000

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
