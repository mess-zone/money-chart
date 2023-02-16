<template>
  <h1>CDI x IPCA</h1>
  <Line :data="data" :options="options" />
</template>

<script setup>
import { computed, onMounted, ref } from 'vue'
import axios from 'axios'
import zoomPlugin from 'chartjs-plugin-zoom';

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

ChartJS.register(zoomPlugin);

const data = computed( () => ({
    // labels: dates.value,
    datasets: [
      {
        label: 'IPCA',
        backgroundColor: 'red',
        data: ipca.value.map(item => ({ x: new Date(item.VALDATA), y: item.VALVALOR})).filter(item => item.x.getFullYear() >= 2018 ).map(item => ({ x: item.x.toISOString().slice(0,10), y: item.y})),
        // data: ipca.value.map(item => ({ x: new Date(item.VALDATA).toISOString().slice(0,10), y: item.VALVALOR})),
      },
      {
        label: 'CDI',
        backgroundColor: 'blue',
        data: cdi.value.map(item => ({ x: new Date(item.VALDATA), y: item.VALVALOR})).filter(item => item.x.getFullYear() >= 2018 ).map(item => ({ x: item.x.toISOString().slice(0,10), y: item.y})),
        // data: cdi.value.map(item => ({ x: new Date(item.VALDATA).toISOString().slice(0,10), y: item.VALVALOR})),
      },

    ]
}))
  
const options = ref({
    // responsive: true,
    // maintainAspectRatio: false
    plugins: {
      zoom: {
        zoom: {
          // wheel: {
          //   enabled: true,
          // },
        //   pinch: {
        //     enabled: true
        //   },
        //   mode: 'x',
        }
      }
    }
  
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
