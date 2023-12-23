<template>
    <div>
      <h3 class="top3">{{ title }}</h3>
      <div v-if="pratosMaisVendidos.length">
        <div v-for="(prato, index) in pratosMaisVendidos" :key="prato.codPrato">
          <p class="produtos">{{ getRankingText(index) }} {{ prato.nomePrato }}: {{ prato.quantidadeVendida }} vendas</p>
        </div>
      </div>
      <div v-else>
        <p>Nenhum dado disponível.</p>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    props: {
      date: {
        type: String,
        required: true,
      },
    },
    data() {
      return {
        title: 'Top Mais Vendidos do Dia',
        pratosMaisVendidos: [],
      };
    },
    watch: {
        date: 'fetchData',
    },
    methods: {
      async fetchData(date) {
        const formattedDate = this.formatDate(this.date);
        try {
          const response = await axios.get(`https://kitchen-back-production.up.railway.app/diario/${formattedDate}`);
          this.pratosMaisVendidos = response.data.pratosMaisVendidos || [];
        } catch (error) {
          console.error('Erro ao buscar dados:', error);
        }
      },
      formatDate(date) {
            // Adicione o T00:00:00 à data se ela não contiver
            return date.includes('T') ? date : `${date}T00:00:00`;
      },
      getRankingText(index) {
        const rankings = ['1º', '2º', '3º', '4º', '5º'];
        return rankings[index] || '';
      },
    },
    mounted() {
      this.fetchData(this.date);
    },
  };
  </script>
  
  <style>
.top3{
    font-family: 'Comic Sans MS';
    color: red;
    font-size: 22px;
    text-align: center;
    font-weight: bold;
    margin-top: -15px;
    margin-bottom: 15px;
}

.produtos{
    font-family: 'Comic Sans MS';
    text-align: left;
    font-size: 18px;
    margin-top: 5px;
    margin-left: 24%;
}
  </style>
  