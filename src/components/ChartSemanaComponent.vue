<template>
    <div>
      <h3>{{ title }}</h3>
      <div style="max-height: 200px; overflow: hidden;">
        <canvas ref="lineChart" width="400" height="300"></canvas>
      </div>
    </div>
  </template>
  
  <script>
import { ref, onMounted } from 'vue';
import axios from 'axios';
import { Chart, LineController, LinearScale, CategoryScale } from 'chart.js';

export default {
  props: {
    date: {
      type: String, // Supondo que a data será uma string no formato yyyy-MM-ddTHH:mm:ss
      required: true,
    },
  },
  watch: {
    date: 'fetchData',
  },
  mounted() {
    this.fetchData(this.date);
  },
  methods: {
    async fetchData(date) {
      const formattedDate = this.formatDate(this.date);
      try {
        this.destroyCharts();
        const response = await axios.get(`https://kitchen-back-production.up.railway.app/diario/${formattedDate}`);
        const data = this.processData(response.data);
        this.createLineChart(data);
      } catch (error) {
        console.error('Erro ao buscar dados:', error);
      }
    },
    formatDate(date) {
      // Adicione o T00:00:00 à data se ela não contiver
      return date.includes('T') ? date : `${date}T00:00:00`;
    },
    destroyCharts() {
      // Destrói os gráficos se existirem
      if (this.chartInstance) {
        this.chartInstance.destroy();
      }
    },
    processData(data) {
      // Organize os dados para o formato necessário
      const daysOfWeek = ['domingo', 'segunda-feira', 'terça-feira', 'quarta-feira', 'quinta-feira', 'sexta-feira', 'sábado'];

      const sortedData = daysOfWeek.map(day => ({
        day,
        sales: data.semana[day] || 0,
      }));

      return sortedData;
    },
    createLineChart(data) {
      const ctx = this.$refs.lineChart.getContext('2d');

      Chart.register(LineController, LinearScale, CategoryScale);

      const chartData = {
        labels: data.map(item => item.day),
        datasets: [
          {
            label: 'Quantidade de Vendas',
            data: data.map(item => item.sales),
            borderColor: 'rgba(255, 0, 0)',
            backgroundColor: 'rgba(255, 0, 0, 0.2)',
            borderWidth: 1,
            fill: false,
          },
        ],
      };

      const options = {
        responsive: true,
        maintainAspectRatio: false,
        scales: {
          y: {
            beginAtZero: true,
          },
        },
        plugins: {
            legend: {
                display: false, // Oculta a legenda
            },
            title: {
                display: true,
                text: 'Quantidade de reservas durante a semana', // Define o título do gráfico
                color: 'rgba(255, 0, 0)',
                font: {
                size: 15,
                family: 'Comic Sans MS'
                },
            },
          },
      };

      this.chartInstance = new Chart(ctx, {
        type: 'line',
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
  