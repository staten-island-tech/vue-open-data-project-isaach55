<template>
    <div class="wrapper">
      <div class="left">
      <SearchBox id="cuisineSearch" :cuisines="cuisineTypes"/>
      <input type="text" placeholder="bronx, queens, etc"/>
      <SearchBox :cuisines="['Bronx', 'Queens', 'Brooklyn']" />
      //TMRW replace cuisines with more general term, make another prop for plaeholder
      <nav class="nav">
        <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/about">About</RouterLink>
      </nav>
      </div>
      <div class="right">
      <h2 class="header">All of these restaurants have rats or mice in them</h2>
      <RouterView />
      </div>
    </div>

</template>

<script setup>
import { ref, onMounted } from 'vue'
import { RouterLink, RouterView } from 'vue-router'
import DataSet from './components/DataSet.vue'
import SearchBox from './components/SearchBox.vue'

const cuisineTypes = ref([])

async function fetchCuisines() {
  const dataURL = `https://data.cityofnewyork.us/resource/43nn-pn8j.json?$select=distinct cuisine_description`
  let res = await fetch(dataURL)
  let cuisineArray = await res.json()
  cuisineTypes.value = cuisineArray.map(item => item.cuisine_description).filter(cuisine => cuisine)
  console.log(cuisineTypes)
}
onMounted(fetchCuisines())

</script>

<style scoped>

.wrapper {
  display: flex;
  flex-direction: row;
  height:100vh;
  width:100vw;
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
}

.header {
  padding: 0 1vw;
}

input[type="text"] {
  width: 100%;
  padding: 8px;
  font-size: 16px;
}

</style>