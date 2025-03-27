<template>
  <div>
    <canvas ref="chartCanvas"></canvas>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue'
import { Chart } from 'chart.js'

// Create a ref for the canvas
const chartCanvas = ref(null)

onMounted(() => {
  // Set up data for your chart
  const data = {
    labels: ['Pizza', 'Chinese', 'Mexican', 'Italian', 'Indian'],
    datasets: [{
      data: [20, 15, 25, 30, 10],
      backgroundColor: ['#FF6384', '#36A2EB', '#FFCE56', '#4BC0C0', '#9966FF'],
      hoverOffset: 4
    }]
  }

  // Set up the chart options
  const options = {
    responsive: true,
    plugins: {
      legend: {
        position: 'top',
      },
      tooltip: {
        callbacks: {
          label: function(tooltipItem) {
            return tooltipItem.label + ': ' + tooltipItem.raw + ' violations';
          }
        }
      }
    }
  }

  // Initialize the Chart.js chart
  new Chart(chartCanvas.value, {
    type: 'pie',  // You can change this to other types (bar, line, etc.)
    data: data,
    options: options
  })
})
</script>

<style scoped>
canvas {
  max-width: 100%;
  height: auto;
}
</style>