<!-- PratosComponent.vue -->

<template>
  <div class="container">
    <div class="component-title2">Mais Vendidos</div>
    <div class="row">
      <div class="column">
        <P class="desc-grafico">Principais mais vendidos</P>
        <bar-chart :data="maisVendidos.principal"></bar-chart>
      </div>
      <div class="column">
        <P class="desc-grafico">Sobremesas mais vendidas</P>
        <bar-chart :data="maisVendidos.sobremesa"></bar-chart>
      </div>
      <div class="column">
        <P class="desc-grafico">Bebidas mais vendidas</P>
        <bar-chart :data="maisVendidos.bebida"></bar-chart>
      </div>
    </div>

    <div class="component-title3">Menos Vendidos</div>
    <div class="row">
      <div class="column">
        <P class="desc-grafico">Principais menos vendidos</P>
        <bar-chart :data="menosVendidos.principal"></bar-chart>
      </div>
      <div class="column">
        <P class="desc-grafico">Sobremesas menos vendidas</P>
        <bar-chart :data="menosVendidos.sobremesa"></bar-chart>
      </div>
      <div class="column">
        <P class="desc-grafico">Bebidas menos vendidas</P>
        <bar-chart :data="menosVendidos.bebida"></bar-chart>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import BarChart from './BarChart.vue';

export default {
  name: 'pratos',
  components: {
    'bar-chart': BarChart,
  },
  data() {
    return {
      maisVendidos: {
        principal: [],
        sobremesa: [],
        bebida: [],
      },
      menosVendidos: {
        principal: [],
        sobremesa: [],
        bebida: [],
      },
      dataLoaded: false, // Nova variável para controlar se os dados foram carregados
    };
  },
  mounted() {
    this.fetchData();
  },
  methods: {
    fetchData() {
      axios.get('https://kitchen-back-production.up.railway.app/pratos/mais-vendidos')
        .then(response => {
          this.maisVendidos = response.data;
          console.log('Mais Vendidos:', this.maisVendidos);
          this.checkDataLoaded(); // Verificar se todos os dados foram carregados
        })
        .catch(error => {
          console.error('Erro ao buscar pratos mais vendidos: ', error);
        });

      axios.get('https://kitchen-back-production.up.railway.app/pratos/menos-vendidos')
        .then(response => {
          this.menosVendidos = response.data;
          console.log('Menos Vendidos:', this.menosVendidos);
          this.checkDataLoaded(); // Verificar se todos os dados foram carregados
        })
        .catch(error => {
          console.error('Erro ao buscar pratos menos vendidos: ', error);
        });
    },
    checkDataLoaded() {
      // Verificar se todos os dados foram carregados
      if (
        this.maisVendidos.principal.length &&
        this.maisVendidos.sobremesa.length &&
        this.maisVendidos.bebida.length &&
        this.menosVendidos.principal.length &&
        this.menosVendidos.sobremesa.length &&
        this.menosVendidos.bebida.length
      ) {
        this.dataLoaded = true;
      }
    },
  },
};
</script>


<style>
.row {
  display: flex;
  justify-content: space-between;
  margin-bottom: 50px;
  margin-top: 10px;
}

.column {
  width: 30%; /* Ajuste a largura conforme necessário */
  height: 320px; /* Ajuste a altura conforme necessário */
  padding: 30px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.component-title {
  width: 100%;
  color: #ff0000a4;
  font-family: 'Lilita One', sans-serif;
  text-align: left;
  margin-right: 950px;
  font-size: 20px;
  margin-top: -20px;
}

.component-title2 {
  width: 200px;
  color: red;
  font-family: 'Lilita One', sans-serif;
  text-align: left;
  margin-left: 450px;
  font-size: 28px;
  margin-top: -150px;
}

.component-title3 {
  width: 200px;
  color: red;
  font-family: 'Lilita One', sans-serif;
  text-align: left;
  margin-left: 450px;
  font-size: 28px;
  margin-top: -20px;
}

.desc-grafico {
  color: rgba(255, 0, 0, 0.815);
  font-family: 'Lilita One', sans-serif;
  font-size: 18px;
  text-align: center;
}
</style>
