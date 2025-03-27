<template>
  <div class="wrapper">
    <div class="left">
      <SearchBox
        id="cuisineSearch"
        :data="cuisineTypes"
        :placeholderText="'What kind of food are you feeling'"
        :type="'cuisine'"
        @searchRestaurant="searchRestaurant"
      />
      <SearchBox
        id="boroughSearch"
        :data="['Bronx', 'Queens', 'Brooklyn', 'Manhattan', 'Staten Island']"
        :placeholderText="'Borough'"
        :type="'borough'"
        @searchRestaurant="searchRestaurant"
      />
      <div class="filterTextContainer">
        <p id="showResultText">showing results for: </p>
          <p class="filterTextBox">{{ cuisineFilter }}</p>
          <p class="filterTextBox">{{ boroughFilter }}</p>
      </div>
      <button @click="restoreDefault" class="filterTextBox">Rat button (clear filters)</button>
      <nav class="nav">
        <RouterLink to="/">Restaurant Search</RouterLink>
        <RouterLink to="/chart">Charts</RouterLink>
      </nav>
    </div>
    <div class="right">
      <h2 class="header">All of these restaurants have rats or mice in them</h2>
      <RouterView />
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, provide } from 'vue'
import { RouterLink, RouterView } from 'vue-router'
import SearchBox from './components/SearchBox.vue'

const cuisineTypes = ref([])
const cuisineFilter = ref('')
const boroughFilter = ref('')
const filteredRestaurantArray = ref([])

provide('cuisineFilter', cuisineFilter)
provide('boroughFilter', boroughFilter)
provide('filteredRestaurantArray', filteredRestaurantArray)

function restoreDefault() {
  cuisineFilter.value = ''
  boroughFilter.value = ''
  filteredRestaurantArray.value = []
}

async function fetchCuisines() {
  const cuisineTypeURL = `https://data.cityofnewyork.us/resource/43nn-pn8j.json?$select=distinct cuisine_description`
  let res = await fetch(cuisineTypeURL)
  let cuisineArray = await res.json()
  cuisineTypes.value = cuisineArray
    .map((item) => item.cuisine_description)
    .filter((cuisine) => cuisine)
  console.log(cuisineTypes)
}
onMounted(fetchCuisines())


function searchRestaurant(query, type) {
  console.log("query: ", query)
  if (type == 'cuisine') {
    cuisineFilter.value = query
  } else {
    boroughFilter.value = query
  }
  fetchRestaurants()
}

async function fetchRestaurants() {
  let restaurantDataURL = ''
  console.log("cuisine filter: ", cuisineFilter.value, "borough filter: ", boroughFilter.value)
  if (cuisineFilter.value && !boroughFilter.value) {
    console.log("creates url")
    restaurantDataURL = `https://data.cityofnewyork.us/resource/43nn-pn8j.json?$where=UPPER(cuisine_description) LIKE '%25${cuisineFilter.value.toUpperCase()}%25' AND (UPPER(violation_description) LIKE '%25RATS%25' OR UPPER(violation_description) LIKE '%25MICE%25')&$limit=200`
  }
  if (!cuisineFilter.value && boroughFilter.value) {
    restaurantDataURL = `https://data.cityofnewyork.us/resource/43nn-pn8j.json?$where=UPPER(boro) LIKE '%25${boroughFilter.value.toUpperCase()}%25' AND (UPPER(violation_description) LIKE '%25RATS%25' OR UPPER(violation_description) LIKE '%25MICE%25')&$limit=200`
  }
  if (cuisineFilter.value && boroughFilter.value) {
    restaurantDataURL = `https://data.cityofnewyork.us/resource/43nn-pn8j.json?$where=UPPER(cuisine_description) LIKE '%25${cuisineFilter.value.toUpperCase()}%25' AND UPPER(boro) LIKE '%25${boroughFilter.value.toUpperCase()}%25' AND (UPPER(violation_description) LIKE '%25RATS%25' OR UPPER(violation_description) LIKE '%25MICE%25')&$limit=200`
  }
  console.log("used url: ", restaurantDataURL)
  let res = await fetch(restaurantDataURL)
  filteredRestaurantArray.value = await res.json()
  console.log(filteredRestaurantArray)
}

</script>

<style scoped>
.wrapper {
  display: flex;
  flex-direction: row;
  height: 100vh;
  width: 100vw;
}

.left {
  display: flex;
  flex-direction: column;
  padding: 1vh 2vh;
}

.right {
  background-color: aqua;
  width: 100%;
  padding: 1vh 0;
}

.nav {
  width: 100%;
  display: flex;
  justify-content: space-between;
}

.header {
  padding: 0 1vw;
}

input[type='text'] {
  width: 100%;
  padding: 8px;
  font-size: 16px;
}

.filterTextContainer {
  display: flex;
  flex-wrap: wrap;
}

.filterTextBox {
  background-color: burlywood;
  margin: 4px;
}

#showResultText {
  margin: 4px;
}

</style>
