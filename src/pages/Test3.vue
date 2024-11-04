<template>
    <Sidebar />
    <Layout>
      <template #link class="pre-barre">
        <Button label="Créer une demande" />
        <Searchbar class="barre" @update:search="updateSearchQuery" />
      </template>
      <template #content-title>
        <h1>Liste des demandes en traitées</h1>
      </template>
      <template #content-subtitle>
        <p>Lorem ipsum dolor sit, amet consectetur adipisicing elit. Atque totam suscipit repellendus, qui sunt tenetur, quia illum rem optio necessitatibus sed perferendis ratione odit aut soluta incidunt unde similique quaerat?</p>
      </template>
      <template #content>
        <div class="content-main">
          <div class="table">
            <!-- Check if there are any results in filteredData -->
            <div v-if="filteredData.length === 0" class="no-results">
              No results found.
            </div>
            <Table v-else :data="filteredData" :labels="tableHeaders" />
          </div>
        </div>
      </template>
    </Layout>
  </template>
  
  <script setup>
  import { ref } from 'vue'
  import Layout from "./../components/Layout.vue"
  import Sidebar from "./../components/Sidebar.vue"
  import Button from "./../components/Button.vue"
  import Searchbar from "./../components/Searchbar.vue"
  import Table from "./../components/Table.vue"
  
  // Table headers and initial data
  const tableHeaders = [
    'Date',
    'Nom Prénom',
    'Rôles',
    'Sites',
    'Résultat de la demande'
  ]
  
  const tableData = [
    { date: '01/01/2024', name: 'DUBOIS Laurent', rights: 'Administrateur', sites: 'Site 1', results: 'Acceptée' },
    { date: '01/02/2024', name: 'HERMANN Margot', rights: 'Administrateur', sites: 'Site 2', results: 'Refusée' },
    //... other rows
  ]
  
  // Reactive properties for filtered data and search query
  const filteredData = ref([])
  const searchQuery = ref('')
  
  // Fetch data based on search query
  const fetchData = async (query) => {
    if (!query.trim()) {
      filteredData.value = tableData
    } else {
      filteredData.value = tableData.filter(data =>
        data.name.toLowerCase().includes(query.toLowerCase())
      )
    }
  }
  
  // Update search query and reload data
  const updateSearchQuery = (query) => {
    console.log('Search query updated:', query)
    searchQuery.value = query
    fetchData(query)
  }
  
  // Load initial data
  fetchData('')
  </script>
  
  <style scoped>
  /* Global CSS */
  body {
    overflow-x: hidden;
  }
  
  body.sidebar-closed .layout {
    margin-left: 60px;
  }
  
  .content-main {
    transition: all 0.3s ease;
    margin-top: 3%;
    display: flex;
    flex-direction: column;
    gap: 20px;
  }
  
  .table {
    flex: 1;
    margin-top: 20px;
  }
  
  .no-results {
    font-size: 1.2em;
    color: #888;
    text-align: center;
    padding: 20px;
  }
  
  body.sidebar-closed .content-main {
    margin-left: 60px;
  }
  
  body.sidebar-closed .navbar nav img {
    display: block;
  }
  
  .navbar nav img {
    display: none;
  }
  
  .layout {
    transition: margin-left 0.3s ease;
    margin-left: 230px;
  }
  </style>
  