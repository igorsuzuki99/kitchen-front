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
  import { Chart, LineController, LinearScale, CategoryScale } from 'chart.js/auto';
  
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
        return date.includes('T') ? date : `${date}T00:00:00`;
      },
      destroyCharts() {
        if (this.chartInstance) {
          this.chartInstance.destroy();
        }
      },
      processData(data) {
        // Organize os dados para o formato necessário
        const hoursOfDay = Array.from({ length: 24 }, (_, index) => index); // Horas de 0 a 23
  
        const sortedData = hoursOfDay.map(hour => ({
          hour,
          sales: data.dia[`${hour.toString().padStart(2, '0')}:00`] || 0,
        }));
  
        return sortedData;
      },
      createLineChart(data) {
        const ctx = this.$refs.lineChart.getContext('2d');
  
        Chart.register(LineController, LinearScale, CategoryScale);
  
        const chartData = {
        labels: data.map(item => {
            const hour = item.hour < 10 ? `0${item.hour}` : item.hour;
            return `${hour}h`;
        }),
          datasets: [
            {
              label: 'Quantidade de Vendas',
              data: data.map(item => item.sales),
              borderColor: 'rgba(255, 0, 0)',
              backgroundColor: 'rgba(255, 0, 0, 0.2)',
              borderWidth: 1,
              fill: false,
              pointRadius: 2,
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
                text: 'Quantidade de reservas durante o dia', // Define o título do gráfico
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
  