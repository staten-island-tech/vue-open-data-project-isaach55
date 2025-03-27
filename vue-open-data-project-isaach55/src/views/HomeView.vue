<script setup>

import { inject } from 'vue'

const cuisineFilter = inject('cuisineFilter')
const boroughFilter = inject('boroughFilter')
const filteredRestaurantArray = inject('filteredRestaurantArray')

</script>

<template>
  <main>
    <img v-if="cuisineFilter == '' && boroughFilter == ''" class="img" src="/bigbacksquad.png" alt="the rats">
    <div v-if="filteredRestaurantArray != []" class="restaurantList">
      <div v-for="restaurant in filteredRestaurantArray" :key="restaurant.id" class="restaurantCard">
        <h2>{{ restaurant.dba }}</h2>
        <p>Cuisine: {{ restaurant.cuisine_description }}</p>
        <p>Borough: {{ restaurant.boro }}</p>
        <p>Address: {{ restaurant.building }} {{ restaurant.street }}, {{ restaurant.zipcode }}</p>
      </div>
    </div>
  </main>
</template>
<style scoped>
.img {
  width: 100%;
}

.restaurantList {
  max-height: 90vh;
  overflow-y: scroll;
  margin: 10px;
  border-radius: 0.5vw;
}

.restaurantCard {
  background-color: white;
  border-radius: 10px;
  padding: 15px;
  margin: 0 10px 10px 0px;
  border: 1px solid #ddd;
  box-sizing: border-box;
}
</style>
