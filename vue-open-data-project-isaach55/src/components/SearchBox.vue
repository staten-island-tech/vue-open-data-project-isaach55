<template>
  <div></div>
</template>

<script setup>
async function fetchCuisines() {
  const dataURL = `https://data.cityofnewyork.us/resource/43nn-pn8j.json?$select=distinct cuisine_description`
  let res = await fetch(dataURL)
  let cuisineArray = await res.json()
  console.log(cuisineArray)
}
fetchCuisines()
</script>

<style scoped></style>
