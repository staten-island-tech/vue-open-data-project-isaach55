<template>
  <div>
    <h2 class="header">NYC Rat Violations by Cuisine</h2>
    <div class="chart-container">
      <ChartComponent v-if="ratRestaurantData.length > 0" 
        class="chart"
        :ratRestaurantData="ratRestaurantData" 
        chartType="pie"
        title="Rat Violations - Pie Chart"
      />
      <ChartComponent v-if="ratRestaurantData.length > 0" 
        class="chart"
        :ratRestaurantData="ratRestaurantData" 
        chartType="bar"
        title="Rat Violations - Bar Chart"
      />
    </div>
  </div>
</template>

<script setup>
import { inject, watchEffect } from 'vue'
import ChartComponent from '@/components/ChartComponent.vue'

const ratRestaurantData = inject('ratRestaurantData')
</script>

<style scoped>
.chart-container {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  gap: 20px;
  align-items: center;
  justify-content: center;

}
.header {
  padding: 0 1vw;
}
.chart {
  width: 40%;
  margin: 3vh 0 0 0;
  height: 60vh;
}
</style>



