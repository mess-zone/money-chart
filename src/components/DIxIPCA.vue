<template>
  <h1>DI x IPCA</h1>
  <pre>
    {{ipca}}
  </pre>
</template>

<script setup>
import { onMounted, ref } from 'vue'
import axios from 'axios'

const cdi = ref([])
const ipca = ref([])

// CDI
async function fetchCDI() {
  const response = await axios.get(`http://ipeadata.gov.br/api/odata4/ValoresSerie(SERCODIGO='BM12_TJCDI12')`);
  return response.data.value
}

// IPCA
async function fetchIPCA() {
  const response = await axios.get(`http://ipeadata.gov.br/api/odata4/ValoresSerie(SERCODIGO='PRECOS12_IPCAG12')`);
  return response.data.value
}

onMounted(async () => {
  cdi.value = await fetchCDI()
  ipca.value = await fetchIPCA()
})
</script>



<style scoped>
</style>
