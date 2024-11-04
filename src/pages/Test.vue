<template>
  <Sidebar />
  <Layout>
    <template #link class="pre-barre">
      <Button label="Créer une demande" @click="redirectToForm()"/>
    </template>
    <template #content-title>
      <h1>DASHBOARD</h1>
    </template>
    <template #content-subtitle>
      <p>Trigoaccess</p>
    </template>
    <template #content>
      <div class="content-main">
        <div class="pie">
          <h2>Pie Chart</h2>
          <PieChart :data="chartData" :labels="chartLabels" />
        </div>

        <div class="box">
          <div class="notification">
            <h2>Notification Box</h2>
            <NotificationBox :items="previewItems" />
          </div>


        </div>

    

          <!-- <h2>Data Table</h2>
          <Table :data="tableData" :labels="tableHeaders" /> -->
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
import PieChart from "./../components/PieChart.vue"
import NotificationBox from "./../components/NotificationBox.vue"
import Table from "./../components/Table.vue"
// Reactive data for the PieChart component

const chartData = ref([5, 3, 2])
const chartLabels = ref(['Demandes cloturées', 'Demandes en cours', 'Demandes en attente'])
// const tableHeaders = ref(['Date', 'Nom Prénom', 'Droits', 'Sites', 'Modifier', 'Etat de la demande'])
// const tableData = ref([
//   ['01/01/2024', 'DUBOIS Laurent', 'Administrateur', 'Site 1', 'Modifier', `En attente d'approbation` ],
//   ['01/02/2024', 'HERMANN Margot', 'Administrateur', 'Site 2', 'Modifier', 'En cours'],
// ])
const previewItems = ref([
  { icon: '✅', label: 'Administrateur - DUBOIS Laurent' },
  { icon: '❌', label: 'Administrateur - HERMANN Margot' },
  { icon: '⚙️', label: 'Test - HOANG William' }
])

const redirectToForm = () => {
    window.location.href = '/main'
}
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
  flex-direction: row;
  gap: 20px;

}

.content-main .box {
  flex: 1;
  display: flex;
  align-items: start;
  justify-content: center;
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
