<template>
  <div class="searchBox">
    <input 
      type="text" 
      v-model="searchQuery" 
      :placeholder= "placeholderText"
    />
    <ul v-if="filteredData.length > 0">
      <li v-for="(data, index) in filteredData" :key="index">
        <button @click="selectQuery(data, type)">{{ data }}</button>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { watch, ref, defineEmits, defineProps } from 'vue'

const props = defineProps({ data: Array, placeholderText: String, type: String })
const emit = defineEmits(['searchRestaurant'])

function selectQuery(data, type) {
  console.log(data)
  console.log(type)
  filteredData.value = []
  searchQuery.value = ''
  emit('searchRestaurant', data, type)
}

const searchQuery = ref("")
const filteredData = ref([])

watch(searchQuery, (newQuery) => {
  if (newQuery) {
    filteredData.value = props.data.filter(data =>
      data.toLowerCase().includes(newQuery.toLowerCase())
    )
  } else {
    filteredData.value = []
  }
})

</script>

<style scoped>

.searchBox {
  position: relative;
  width: 20vw;
  max-height: 40%;
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
