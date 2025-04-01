<template>
    <div>
      <h3>{{ title }}</h3>
      <Pie v-if="chartType === 'pie'" :data="chartData" :options="chartOptions" />
      <Bar v-if="chartType === 'bar'" :data="chartData" :options="chartOptions" />
    </div>
  </template>
  
  <script setup>
  import { defineProps, computed } from 'vue'
  import { Pie, Bar } from 'vue-chartjs'
  import {
    Chart as ChartJS,
    Title,
    Tooltip,
    Legend,
    CategoryScale,
    LinearScale,
    BarElement,
    ArcElement
  } from 'chart.js'
  
  ChartJS.register(Title, Tooltip, Legend, CategoryScale, LinearScale, BarElement, ArcElement)
  
  const props = defineProps({
    ratRestaurantData: Array,
    chartType: String,
    title: String
  })
  
  // computes violation count
  const chartData = computed(() => {
    const cuisineCounts = props.ratRestaurantData.reduce((acc, restaurant) => {
      const cuisine = restaurant.cuisine_description
      if (cuisine) {
        acc[cuisine] = (acc[cuisine] || 0) + 1
      }
      return acc
    }, {})
  
    const total = Object.values(cuisineCounts).reduce((sum, count) => sum + count, 0)
  
    let labels = Object.keys(cuisineCounts)
    let data = Object.values(cuisineCounts)
  
    const sortedCuisineCounts = Object.entries(cuisineCounts)
      .sort((a, b) => b[1] - a[1]) // order by quantity
  
    // too many categories, so take top 8 and separate the rest
    const topCuisineCounts = sortedCuisineCounts.slice(0, 8)
    const otherCuisineCount = sortedCuisineCounts.slice(8).reduce((sum, item) => sum + item[1], 0)
  
    // making one item in the array to represent all "other" categories
    if (otherCuisineCount > 0) {
      topCuisineCounts.push(['Others', otherCuisineCount])
    }
  
    labels = topCuisineCounts.map(item => item[0])
    data = topCuisineCounts.map(item => item[1])

    return {
      labels,
      datasets: [
        {
          label: 'Violations',
          data,
          backgroundColor: ['#FF6384', '#36A2EB', '#FFCE56', '#4BC0C0', '#9966FF', '#FF5733', '#C70039', '#900C3F', '#581845', '#DAF7A6'],
          hoverOffset: 4
        }
      ]
    }
  })
  
  const chartOptions = {
    responsive: true,
    plugins: {
      legend: {
        position: 'top'
      },
      tooltip: {
        callbacks: {
          label: (tooltipItem) => {
            const percentage = ((tooltipItem.raw / chartData.value.datasets[0].data.reduce((a, b) => a + b, 0)) * 100).toFixed(2)
            return `${tooltipItem.label}: ${percentage}%`
          }
        }
      },
      datalabels: {
        display: true,
        color: 'white',
        formatter: (value) => {
          const percentage = ((value / chartData.value.datasets[0].data.reduce((a, b) => a + b, 0)) * 100).toFixed(1)
          return `${percentage}%`
        }
      }
    }
  }
  </script>
  
  <style scoped>
  h3 {
    text-align: center;
    margin-bottom: 10px;
  }
  </style>
  