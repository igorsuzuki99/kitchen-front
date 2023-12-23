<template>
    <div class="chart-container">
      <canvas id="chartBar" ref="barChart" width="550" height="200"></canvas>
    </div>
  </template>
  
  <script>
  import { Chart } from 'chart.js/auto';
  import axios from 'axios';
  
  export default {
    name: 'BarChartAtendimentosComponent',
    props: {
      dataInicio: String,
      dataFim: String,
    },
    data() {
        return {
            dataAvaliacaoIni: '2023-09-01T00:00:00',
            dataAvaliacaoFin: '2023-09-30T00:00:00',
        };
    },
    mounted() {
      this.fetchDataAndCreateBarChart(this.dataAvaliacaoIni, this.dataAvaliacaoFin);
    },
    methods: {
      fetchDataAndCreateBarChart(dataAvaliacaoIni, dataAvaliacaoFin) {
        if (this.barChart) {
            this.barChart.destroy();
        }
        axios.get('https://kitchen-back-production.up.railway.app/funcionarios/media-notas', {
          params: {
            dataAvaliacaoIni: dataAvaliacaoIni,
            dataAvaliacaoFin: dataAvaliacaoFin,
          }
        })
          .then(response => {
            const data = response.data;
            this.createBarChart(data);
          });
      },
      createBarChart(data) {
        const ctx = this.$refs.barChart.getContext('2d');
        const chartData = {
          labels: data.map(item => item.nomeFuncionario),
          datasets: [
            {
              label: 'Média de Avaliação',
              data: data.map(item => item.mediaAvalicao/2),
              backgroundColor: 'rgba(255, 0, 0, 0.2)', // Cor vermelha
              borderColor: 'rgba(219,112,147)', // Cor vermelha
              borderWidth: 1,
              barThickness: 40,
            },
          ],
        };
  
        const options = {
          responsive: true,
          maintainAspectRatio: false,
          scales: {
            y: {
              beginAtZero: true,
              max: 5, // Defina o valor máximo desejado para o eixo Y
            },
          },
          plugins: {
            title: {
              display: true,
              text: 'Média Avaliações',
              color: 'red',
              font: {
                size: 20,
                family: 'Arial',
              },
            },
          },
        };
  
        this.barChart = new Chart(ctx, {
          type: 'bar',
          data: chartData,
          options,
        });
      },
    },
  };
  </script>

<style scoped>
.chart-container {
  border: 1px solid #ff0000; /* Adicione o estilo da borda desejada */
  padding: 10px; /* Ajuste o preenchimento conforme necessário */
  display: flex; /* Mantenha o gráfico em linha com a borda */
  margin-left: 30px;
}
</style>
  