<template>
  <div class="searchBox">
    <input 
      type="text" 
      v-model="searchQuery" 
      placeholder="What kind of food are you feeling" 
    />
    <ul v-if="filteredCuisines.length > 0">
      <li v-for="(cuisine, index) in filteredCuisines" :key="index">
        <button @click="selectCuisine(cuisine)">{{ cuisine }}</button>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { watch, ref } from 'vue'

const props = defineProps({ cuisines: Array })

const searchQuery = ref("")
const filteredCuisines = ref([])

watch(searchQuery, (newQuery) => {
  if (newQuery) {
    filteredCuisines.value = props.cuisines.filter(cuisine =>
      cuisine.toLowerCase().includes(newQuery.toLowerCase())
    )
  } else {
    filteredCuisines.value = []
  }
})

</script>

<style scoped>

.searchBox {
  position: relative;
  width: 20vw;
  max-height: 70%;
}

input[type="text"] {
  width: 100%;
  padding: 8px;
  font-size: 16px;
  margin-bottom: 8px;
}

ul {
  list-style-type: none;
  padding: 0;
  margin: 0;
  border: 1px solid #ccc;
  max-height: 85%;
  overflow-y: auto;
}

li {
  padding: 8px;
  cursor: pointer;
}

button {
  width: 100%;
  padding: 8px;
  font-size: 16px;
  border: none;
  background-color: transparent;
  text-align: left;
  cursor: pointer;
  box-sizing: border-box; 
}

button:hover {
  background-color: #f0f0f0;
}

</style>
