<template>

    <Sidebar />
    <Layout>
        <template #link class="pre-barre">
            <div class="centered-searchbar">
                <Button label="Créer une demande" @click="redirectToForm()" />
                <!-- <Button label="Modifier une demande" @click="showModal2 = true"/> -->
                <Searchbar class="barre" @update:search="updateSearchQuery" />
            </div>
        </template>
        <template #content-title>
            <h1>Liste des demandes en cours</h1>
        </template>
        <template #content-subtitle>
            <p>Lorem ipsum dolor sit amet consectetur, adipisicing elit. Ducimus dolorum neque ratione, sunt ab modi
                molestiae. Asperiores ipsa sed, earum excepturi, pariatur repudiandae ut reiciendis qui molestiae
                consectetur obcaecati consequuntur?</p>
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

    <!-- Utilisation modale de création de demande  -->

    <!-- <Modal v-if="showModal1" :visible="showModal1" title="Création d'une requête" @close="showModal1 = false">
        <template #modal-content>
            <p class="modal-p">Veuillez entrer les informations du concerné</p>
            <form>
                <div class="part1-form">

                    <InputText inputname="Nom" />
                    <InputText inputname="Prénom" />
                    <InputText inputname="Manager" />




                </div>
            </form>
        </template>


    </Modal> -->
   
</template>

<script setup>
import { ref } from 'vue'
import Layout from "./../components/Layout.vue"
import Sidebar from "./../components/Sidebar.vue"
import Button from "./../components/Button.vue"
import Searchbar from "./../components/Searchbar.vue"
import Table from "./../components/Table.vue"
import Modal from "./../components/Modal.vue"
import InputText from "./../components/InputText.vue"
// Table headers and initial data
const tableHeaders = [
    'Date',
    'Nom Prénom',
    'Rôles',
    'Sites',
    'Actions',
    'État de la demande'
]

const tableData = [
    { date: '01/01/2024', name: 'DUBOIS Laurent', rights: 'Administrateur', sites: 'Site 1', actions: 'Modifier', states: 'Approuvée' },
    { date: '01/02/2024', name: 'HERMANN Margot', rights: 'Administrateur', sites: 'Site 2', actions: 'Modifier', states: 'Refusée' },
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

// Utilisation modale de création de demande  -->
// const showModal1 = ref(false)
// const showModal2 = ref(false)

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

/* Centering the Searchbar */
.centered-searchbar {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 10px;
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

.modal-p {
    padding-top: 1%;
    font-size: 16px;
    text-align: center;
    color: grey;
}
</style>