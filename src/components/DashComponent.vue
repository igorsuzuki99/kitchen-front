<template>
  <div class="container">
    <div class="component-title">
      <h1>Visão Geral do Dia {{ formattedDate }}</h1>
    </div>
    <div class="input-wrapper">
      <input type="date" v-model="selectedDate"/>
    </div>

    <div class="row2">
      <div class="column2">
        <ChartHoras :date="selectedDate" title="Atendimento durante o dia"></ChartHoras>
      </div>
      <div class="column2">
        <ChartSemana :date="selectedDate" title="Atendimentos durante a semana"></ChartSemana>
      </div>
    </div>
    <div class="row2">
      <div class="column3">
        <TableFuncionarios :date="selectedDate"></TableFuncionarios>
      </div>
      <div class="column2">
        <TopProdutos :date="selectedDate"></TopProdutos>
      </div>
    </div>
    

  </div>
</template>

<script>
import ChartEstoqueComponent from './ChartEstoqueComponent.vue';
import ChartVendasComponent from './ChartVendasComponent.vue';
import ChartSemana from './ChartSemanaComponent.vue';
import ChartHoras from './ChartHorasComponent.vue';
import TableFuncionarios from './TableDiaFuncionariosComponent.vue';
import TopProdutos from './TopVendasComponent.vue';
import axios from 'axios';

export default {
  name: 'painel',
  components: {
    ChartEstoqueComponent, ChartVendasComponent, ChartSemana, ChartHoras, TableFuncionarios, TopProdutos
  },
  data() {
    return {
      showChart: true, // Defina como true para mostrar o gráfico automaticamente
      estoqueData: [],
      vendasData: [],
      selectedDate: null,
      dadosDoEndpoint: {},
    };
  },
  computed: {
    formattedDate() {
      // Formate a data escolhida como "dd/MM/yyyy" (levando em consideração o fuso horário local)
      if (this.selectedDate) {
        const date = new Date(`${this.selectedDate}T00:00:00`);
        const options = { day: '2-digit', month: '2-digit', year: 'numeric' };
        return date.toLocaleDateString(undefined, options);
      }
      return '';
    },
  },
  methods: {
    updateData() {
      this.$refs.chartHoras.fetchData(this.selectedDate);
      this.$refs.chartSemana.fetchData(this.selectedDate);
      this.$refs.tableFuncionarios.fetchData(this.selectedDate);
      this.$refs.topProdutos.fetchData(this.selectedDate);
    },
  }
};
</script>

<style>
/* Estilos para os cards */
.card-container {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 20px;
}

.card {
  width: 550px;
  height: 250px;
  border: 1px solid #ff0000;
  margin-top: 30px;
}

.input-wrapper {
  margin-left:88%;
  margin-top: 1%;
}

.row2 {
  display: flex;
  justify-content: center;
  margin-bottom: 50px;
  margin-top: 20px;
}

.column2 {
  width: 45%; /* Ajuste a largura conforme necessário */
  height: 250px; /* Ajuste a altura conforme necessário */
  padding: 30px;
  border: 1px solid #ddd;
  border-radius: 10px;
  border-color: red;
  margin-left: 30px;
}

.column3 {
  width: 45%; /* Ajuste a largura conforme necessário */
  height: 250px; /* Ajuste a altura conforme necessário */
  padding: 30px;
  margin-left: 30px;
}
</style>
