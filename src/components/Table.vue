<template>
    <div class="table-container">
      <table>
        <thead>
          <tr>
            <th v-for="label in labels" :key="label">{{ label }}</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(row, index) in data" :key="index">
            <td v-for="(value, i) in row" :key="i">{{ value }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </template>
  
  <script setup>
  import { defineProps } from 'vue'
  
  const props = defineProps({
    data: {
      type: Array,
      required: true,
      default: () => []
    },
    labels: {
      type: Array,
      required: true,
      default: () => []
    }
  })
  </script>
  
  <style scoped>
  .table-container {
    margin: 20px 0;
    overflow-x: auto;
  }
  table {
    width: 100%;
    border-collapse: collapse;
  }
  th, td {
    padding: 12px;
    border: 1px solid #ddd;
    text-align: left;
  }
  thead {
    background-color: #f4f4f4;
  }
  </style>
  