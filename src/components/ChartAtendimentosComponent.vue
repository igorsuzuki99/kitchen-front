<template>
    <div class="chart-container">
      <canvas ref="chart" width="550" height="200"></canvas>
    </div>
</template>
  
  <script>
  import { Chart } from 'chart.js/auto';
  import axios from 'axios';
  
  export default {
    name: 'ChartAtendimentosComponent',
    mounted() {
      this.fetchDataAndCreateChart(this.data1, this.data2);
    },
    methods: {
      fetchDataAndCreateChart(data1, data2) {
        if (this.chart) {
          this.chart.destroy();
        }
        axios.get('https://kitchen-back-production.up.railway.app/funcionarios/total-atendimentos', {
          params: {
            data1: data1,
            data2: data2,
          }
        })
          .then(response => {
            const atendimentosData = response.data;
  
            this.createChart(atendimentosData);
          });
      },
      createChart(data) {
        const ctx = this.$refs.chart.getContext('2d');
        const chartData = {
          labels: Object.keys(data),
          datasets: [
            {
              data: Object.values(data),
              backgroundColor: [
                'rgba(255, 99, 132, 0.6)',
                'rgba(54, 162, 235, 0.6)',
                'rgba(255, 206, 86, 0.6)',
                'rgba(75, 192, 192, 0.6)',
              ],
              borderColor: 'rgba(219,112,147)', // Defina a cor da borda aqui
              borderWidth: 1,
            },
          ],
        };
  
        const options = {
          responsive: true,
          maintainAspectRatio: false,
          plugins: {
            title: {
              display: true,
              text: 'Atendimentos Funcionários',
              color: 'red',
              font: {
                size: 20,
                family: 'Arial',
              },
            },
            legend: {
            display: true,
            position: 'right',
            labels: {
              generateLabels: function (chart) {
                const data = chart.data;
                if (data.labels.length && data.datasets.length) {
                  return data.labels.map(function (label, i) {
                    const meta = chart.getDatasetMeta(0);
                    const ds = data.datasets[0];
                    const currentValue = ds.data[i];
                    return {
                      text: `${label}: ${currentValue.toFixed(0)}%`,
                      fillStyle: meta.controller.getStyle(i).backgroundColor,
                      strokeStyle: 'transparent',
                      lineWidth: 2,
                      hidden: isNaN(currentValue),
                      index: i,
                    };
                  });
                }
                return [];
              },
            },
          },
          },
        };
  
        this.chart = new Chart(ctx, {
          type: 'pie',
          data: chartData,
          options,
        });
      },
    },
    data() {
      return {
        data1: '2023-09-01T00:00:00',
        data2: '2023-09-30T00:00:00',
      };
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
  