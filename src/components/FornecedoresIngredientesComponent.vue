<template>
    <div>
      <div class="component-nome">
        <H1>Fornecedores do Ingrediente {{ codIngrediente }}</H1>
      </div>

      <table>
        <thead>
          <tr>
            <th>Razão Social</th>
            <th>Categoria</th>
            <th>Nome Fornecedor</th>
            <th>Custo frete</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="fornecedor in fornecedores" :key="fornecedor.codFornecedor">
            <td>{{ fornecedor.razaoSocial }}</td>
            <td>{{ fornecedor.categoria }}</td>
            <td>{{ fornecedor.prazoEntrega }}</td>
            <td>R$ {{ fornecedor.custoFrete }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    props: {
      codIngrediente: {
        type: Number,
        required: true,
      },
    },
    data() {
      return {
        fornecedores: [],
      };
    },
    created() {
      this.fetchFornecedores();
    },
    methods: {
        async fetchFornecedores() {
            try {
            const response = await axios.get(`https://kitchen-back-production.up.railway.app/ingrediente/fornecedores-ingredientes?idIngrediente=${this.codIngrediente}`);
            console.log(response.data);
            this.fornecedores = response.data;
            } catch (error) {
            console.error('Erro ao buscar fornecedores:', error);
            }
        },
        },
  };
  </script>
  