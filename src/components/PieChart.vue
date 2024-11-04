<template>
    <div class="chart-container">
      <Pie :data="chartData" :options="chartOptions" />
    </div>
  </template>
  
  <script>
  import { Pie } from 'vue-chartjs'
  import { Chart as ChartJS, Title, Tooltip, Legend, ArcElement } from 'chart.js'
  
  // Register required chart components
  ChartJS.register(Title, Tooltip, Legend, ArcElement)
  
  export default {
    name: 'PieChart',
    components: { Pie },
    props: {
      data: {
        type: Array,
        required: true,
      },
      labels: {
        type: Array,
        required: true,
      }
    },
    data() {
      return {
        chartData: {
          labels: this.labels,
          datasets: [
            {
              backgroundColor: ['#FF6384', '#36A2EB', '#FFCE56'],
              data: this.data
            }
          ]
        },
        chartOptions: {
          responsive: true,
          maintainAspectRatio: false
        }
      }
    },
    watch: {
      data(newData) {
        this.chartData.datasets[0].data = newData
      },
      labels(newLabels) {
        this.chartData.labels = newLabels
      }
    }
  }
  </script>
  
  <style scoped>
  .chart-container {
    position: relative;
    height: 50vh;
    width: 50vw;
  }
  </style>
  