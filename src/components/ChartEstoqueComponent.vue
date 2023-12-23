<template>
    <div>
      <canvas ref="chart" width="400" height="600"></canvas>
    </div>
  </template>
  
  <script>
import { Chart } from 'chart.js/auto';
import axios from 'axios';

export default {
  mounted() {
    this.fetchDataAndCreateChart();
  },
  methods: {
    fetchDataAndCreateChart() {
      axios.get('https://kitchen-back-production.up.railway.app/view-estoque')
        .then(response => {
          const estoqueData = response.data.map(item => ({
            nomeIngrediente: item.nomeIngrediente,
            quantidade: item.quantidade,
          }));
          this.createChart(estoqueData);
        })
        .catch(error => {
          console.error('Erro ao buscar dados do estoque: ', error);
        });
    },
    createChart(data) {
      const ctx = this.$refs.chart.getContext('2d');
      const chartData = {
        labels: data.map(item => item.nomeIngrediente),
        datasets: [
          {
            label: 'Quantidade',
            backgroundColor: 'rgba(255, 0, 0, 0.2)', // Cor vermelha
            borderColor: 'rgba(255, 0, 0, 1)', // Cor vermelha
            borderWidth: 1,
            data: data.map(item => item.quantidade),
          },
        ],
      };

      const options = {
        responsive: true,
        maintainAspectRatio: false,
        plugins: {
            title: {
            display: true,
            text: 'Gestão de Estoque',
            color: 'red',
            font: {
                size: 20,
                family: 'Arial'
            },
            },
        },
        scales: {
          y: {
            beginAtZero: true,
            title: {
              display: true,
              text: 'Quantidade',
            },
            ticks: {
              stepSize: 1,
              maxTicksLimit: 10,
            },
          },
          x: {
            title: {
              display: true,
              text: 'Ingrediente',
            },
          },
        },
      };

      // Ajuste a altura do gráfico para corresponder à altura do elemento canvas
      options.layout = {
        padding: {
          left: 20,
          right: 20,
          top: 20,
          bottom: 20,
        },
      };

      new Chart(ctx, {
        type: 'bar',
        data: chartData,
        options,
      });
    },
  },
};
</script>
