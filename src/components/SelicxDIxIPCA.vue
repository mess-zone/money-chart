<template>
  <h1>Selic x CDI x IPCA (variação mensal)</h1>
  <Line :data="data" :options="options" />
</template>

<script setup>
import { computed, onMounted, ref } from 'vue'
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
  Legend,
)

const data = computed( () => ({
    datasets: [
      {
        label: 'IPCA',
        backgroundColor: 'red',
        data: ipca.value,
      },
      {
        label: 'CDI',
        backgroundColor: 'blue',
        data: cdi.value,
      },
      {
        label: 'Selic',
        backgroundColor: 'orange',
        data: selic.value,
      },

    ]
}))
  
const options = ref({
    // responsive: true,
    // maintainAspectRatio: false
  
})
  

const selic = ref([])
const cdi = ref([])
const ipca = ref([])

// Selic
async function fetchSelic() {
  return fetchData('https://api.bcb.gov.br/dados/serie/bcdata.sgs.4390/dados?formato=json')
}

// CDI
async function fetchCDI() {
  return fetchData('https://api.bcb.gov.br/dados/serie/bcdata.sgs.4391/dados?formato=json')
}

// IPCA
async function fetchIPCA() {
  return fetchData('https://api.bcb.gov.br/dados/serie/bcdata.sgs.433/dados?formato=json')
}

async function fetchData(url) {
  return axios.get(url)
  .then(response => response.data)
  .then(data => {
    return data
      .map(item => ({ x: stringToDate(item.data), y: item.valor }))
      .filter(item_1 => item_1.x.getFullYear() >= 2018)
      .map(item_2 => ({ x: item_2.x.toISOString().slice(0, 10), y: item_2.y }));
  })
}

function stringToDate(string) {
  const [day, month, year] = string.split('/')
  return new Date(`${year}-${month}-${day}`)
}

onMounted(async () => {
  selic.value = await fetchSelic()
  cdi.value = await fetchCDI()
  ipca.value = await fetchIPCA()
})
</script>



<style scoped>
</style>
