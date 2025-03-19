<template>
  <div>
    <input type="text" id="filterInput" name="" placeholder="What kind of food are you feeling" />
  </div>
</template>

<script setup>
async function fetchCuisines() {
  const dataURL = `https://data.cityofnewyork.us/resource/43nn-pn8j.json?$select=distinct cuisine_description`
  let res = await fetch(dataURL)
  let cuisineArray = await res.json()
  console.log(cuisineArray)
}
fetchCuisines()

let inputBox = document.getElementById('filterInput')
let cuisines = []
let search = ''

function autocomplete(input, array) {
  input.addEventListener("input", function( {
    cuisines = []

  }
)

  }

autocomplete(inputBox, cuisineArray)
</script>

<style scoped></style>
