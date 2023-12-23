<!--<template>
  <div class="container">
    <div class="component-title">
      <h1>DASHBOARD</h1>
    </div>

    <ChartEstoqueComponent v-if="showChart" :estoqueData="estoqueData" />
    <div class="input-wrapper">
      <input type="date" v-model="selectedDate" @input="handleDateInputChange" />
    </div>
    <ChartVendasComponent v-if="showChart" :vendasData="vendasData" :selectedDate="selectedDate" />

  </div>
</template>

<script>
import ChartEstoqueComponent from './ChartEstoqueComponent.vue';
import ChartVendasComponent from './ChartVendasComponent.vue'

export default {
  name: 'painel',
  components: {
    ChartEstoqueComponent, ChartVendasComponent // Registre o componente
  },
  data() {
    return {
      showChart: true, // Defina como true para mostrar o gráfico automaticamente
      estoqueData: [],
      vendasData: [], // Seus dados de estoque
      selectedDate: null,
    };
  },
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
  margin-top: 5%;
}
</style> -->
