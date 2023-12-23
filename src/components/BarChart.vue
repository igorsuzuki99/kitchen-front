<!-- BarChart.vue -->

<template>
    <div>
      <h3>{{ title }}</h3>
      <div style="max-height: 500px; overflow: hidden;">
        <canvas ref="chart" width="300"></canvas>
      </div>
    </div>
  </template>
  
  <script>
  import { Chart, BarController, LinearScale, CategoryScale } from 'chart.js';
  
  export default {
    props: {
      data: {
        type: Array,
        required: true,
      },
      title: {
        type: String,
        required: true,
      },
    },
    watch: {
      data: {
        immediate: true,
        handler(newData) {
          if (newData.length) {
            this.createChart();
          }
        },
      },
    },
    mounted() {
      // Remova a chamada para createChart do mounted, pois agora ela é acionada pelo watcher
    },
    methods: {
      createChart() {
        const ctx = this.$refs.chart.getContext('2d');
  
        Chart.register(BarController, LinearScale, CategoryScale);
  
        const chartData = {
          labels: this.data.map(item => item.nomePrato),
          datasets: [
            {
              label: 'Vendas',
              data: this.data.map(item => item.quantidade),
              backgroundColor: 'rgba(255, 0, 0, 0.2)',
              borderColor: 'rgba(219,112,147)',
              borderWidth: 1,
              barThickness: 30,
              maxBarThickness: 30,
            },
          ],
        };
  
        const options = {
          responsive: false, // Desativando responsividade
          maintainAspectRatio: false,
          scales: {
            y: {
              beginAtZero: true,
            },
            x: {
                barPercentage: 0.5,
                maxBarThickness: 50, 
                maxRotation: 0,
                autoSkip: true,
            },
          },
        };

        this.$refs.chart.width = 250; 
        this.$refs.chart.height = 250; 
  
        new Chart(ctx, {
          type: 'bar',
          data: chartData,
          options,
        });
      },
    },
  };
  </script>
  

  
  <style>
  /* Estilos opcionais do componente */
  </style>
  