<template>
  <h1>CDI x IPCA</h1>
  <Line :data="data" :options="options" />

  <div style="display: flex">
    <pre>
      <h2>CDI</h2>
      {{cdi}}
    </pre>
    <pre>
      <h2>IPCA</h2>
      {{ipca}}
    </pre>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue'
import axios from 'axios'

import {
  Chart as ChartJS,
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Title,
  Tooltip,
  Legend
} from 'chart.js'
import { Line } from 'vue-chartjs'

ChartJS.register(
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Title,
  Tooltip,
  Legend
)

const data = ref({
    labels: ['01/23', '02/23', '03/23', '04/23', '05/23', '06/23', '07/23'],
    datasets: [
      {
        label: 'CDI',
        backgroundColor: 'blue',
        data: [40, 39, 10, 40, 39, 80, 40]
      },
      {
        label: 'IPCA',
        backgroundColor: 'red',
        data: [20, 10, 40, 20, 45, 56, 78]
      }
    ]
})
  
const options = ref({
    // responsive: true,
    // maintainAspectRatio: false
})
  

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
