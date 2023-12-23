<template>
    <head>
        <link
        rel="stylesheet"
        href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.2/css/all.min.css"/>
    </head>
    <div class="container">
        <div class="component-title">
            <H1>OPERAÇÕES</H1>
        </div>
        <div class="button-container">
            <button @click="setActiveTab('compra')">Registrar Compra</button>
            <button @click="setActiveTab('entrada')">Entrada estoque</button>
            <button @click="setActiveTab('saida')">Saída estoque</button>
            <button @click="setActiveTab('alerta')">Alerta</button>
        </div>
        <registra-compra v-if="activeTab === 'compra'"></registra-compra>
        <entrada-estoque v-if="activeTab === 'entrada'"></entrada-estoque>
        <saida-estoque v-if="activeTab === 'saida'"></saida-estoque>
        <alerta v-if="activeTab === 'alerta'"></alerta>


    </div>
</template>

<script>
import axios from 'axios';

import compra from './RegistrarCompraComponent.vue';
import entrada from './EntradaComponent.vue';
import saida from './SaidaComponent.vue';
import alerta from './AlertaComponent.vue';

export default {
  name: 'operacoes',
  data() {
    return {
        activeTab: 'compra',
    };
  },
  components: {
    'registra-compra': compra,
    'entrada-estoque': entrada,
    'saida-estoque': saida,
    'alerta': alerta,
  },
  computed: {
    
  },
  mounted() {
    
  },
  methods: {
    setActiveTab(tabName) {
        this.activeTab = tabName;
      },
  },
};
</script>

<style>
.container {
  display: flex;
  flex-direction: column;
  align-items: stretch; /* Faz com que o componente ocupe todo o espaço disponível na vertical */
  justify-content: stretch; /* Faz com que o componente ocupe todo o espaço disponível na horizontal */
  height: 100vh; /* Define a altura para ocupar 100% da altura da tela */
  width: 100%; /* Define a largura para ocupar 100% da largura disponível */
  padding: 20px;
  margin-top: 100px;
  margin-bottom: 16rem;
  color: black;
}

.button-container {
    margin-bottom: 40px;
}

.button-container button:hover {
    background-color: rgba(254, 0, 0, 0.153);
}
</style>