<template>
  <div>
    <canvas ref="chart" width="400" height="600"></canvas>
  </div>
</template>
  
<script>
import { Chart } from 'chart.js/auto';
import axios from 'axios';

export default {
  props: {
    vendasData: Array,
    selectedDate: String,
  },

  watch: {
    selectedDate: 'filterDataByDate',
  },
  mounted() {
    // Adicione essa verificação para garantir que o gráfico só seja destruído quando já foi criado
    if (this.chart) {
      this.chart.destroy();
    }
    this.fetchDataAndCreateChart();
  },
  data() {
    return {
      chart: null,
      chartData: [],
      filteredChartData: [],
    };
  },
  methods: {
    handleDateInputChange() {
      console.log('Input date changed:', this.selectedDate);
    },
    fetchDataAndCreateChart() {
      axios.get('https://kitchen-back-production.up.railway.app/vendas')
        .then(response => {
          const vendasData = response.data.map(item => ({
            nomePrato: item.nomePrato,
            quantidade: item.quantidade,
            dataVenda: item.dataVenda,
          }));
          this.chartData = vendasData;
          this.filteredChartData = vendasData;
          this.createChart();
        })
        .catch(error => {
          console.error('Erro ao buscar dados de venda: ', error);
        });
    },
    filterDataByDate() {
      if (!this.selectedDate) {
        this.filteredChartData = this.chartData;
      } else {
        const selectedDate = new Date(this.selectedDate).toISOString().split('T')[0];
        this.filteredChartData = this.chartData.filter(item => {
          const itemDate = item.dataVenda.split('T')[0];
          return itemDate === selectedDate;
        });
      }

      if (this.chart) {
        this.chart.destroy();
      }
      this.createChart();
    },

    createChart(data) {
      const ctx = this.$refs.chart.getContext('2d');
      const chartData = {
        labels: this.filteredChartData.map(item => item.nomePrato),
        datasets: [
          {
            label: 'Quantidade',
            backgroundColor: 'rgba(255, 0, 0, 0.2)',
            borderColor: 'rgba(255, 0, 0, 1)',
            borderWidth: 1,
            data: this.filteredChartData.map(item => item.quantidade),
          },
        ],
      };

      const options = {
        responsive: true,
        maintainAspectRatio: false,
        plugins: {
          title: {
            display: true,
            text: 'Gestão de Vendas',
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
              text: 'Prato',
            },
          },
        },
      };

      options.layout = {
        padding: {
          left: 20,
          right: 20,
          top: 20,
          bottom: 20,
        },
      };

      this.chart = new Chart(ctx, {
        type: 'bar',
        data: chartData,
        options,
      });
    },
  },
};
</script>
