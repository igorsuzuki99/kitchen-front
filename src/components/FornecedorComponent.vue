<template>
    <div class="search-input">
        <input placeholder="Pesquisar fornecedor" class="input-search" v-model="searchTerm">
    </div>

    <table class="table-data">
        <thead>
        <tr>
            <th>Razão Social</th>
            <th>Categoria</th>
            <th>Localização</th>
            <th>Prazo entrega</th>
            <th>CNPJ</th>
            <th>Telefone</th>
        </tr>
        </thead>
        <tbody>
            <tr v-for="item in filteredFornecedor" :key="item.cnpj">
                <td>{{ item.razaoSocial }}</td>
                <td>{{ item.categoria }}</td>
                <td>{{ item.localizacao }}</td>
                <td>{{ item.prazoEntrega }}</td>
                <td>{{ item.cnpj }}</td>
                <td>{{ item.telefone }}</td>
            </tr>
        </tbody>
    </table>
</template>

<script>
import axios from 'axios';
export default {
name: 'fornecedor',
data() {
  return {
    fornecedorData: [],
    searchTerm: '',
  };
},
computed: {
  filteredFornecedor() {
    const termo = this.searchTerm.toLowerCase();
    return this.fornecedorData.filter(item => item.cnpj.toLowerCase().includes(termo));
  }
},

mounted() {
  this.fetchData();
},
methods: {
  fetchData() {
    axios.get('https://kitchen-back-production.up.railway.app/fornecedores')
    .then(response => {
      this.fornecedorData = response.data;
    })
    .catch(error => {
      console.error('Erro ao realizar busca nos fornecedores: ', error);
    });
  }
}

}
</script>

<style>
.search-input {
display: flex;
justify-content: flex-end;
margin-bottom: 10px;
margin-top: 50px; 
margin-right: -30px;
}
.input-search {
padding: 5px;
border: 1px solid #ddd;
border-radius: 4px;
font-family: 'Times New Roman', sans-serif;
width: 25%; /* Define a largura para ocupar 100% do espaço disponível */
}
.search-button {
margin-left: 5px;
border-radius: 4px;
border: 1px solid #00000052;
background-color: #ff000065;
color: white;
}

input {
padding: 5px;
border: 1px solid #ddd;
border-radius: 4px;
font-family: 'Times New Roman', sans-serif;
}

table {
  width: 100%;
  border-collapse: collapse;
  text-align: center;
  font-family: 'Times New Roman', sans-serif;
  margin-top: 10px;
  margin-left: 30px;
}

table th {
  background-color: #ffc6c6;
  border: 1px solid #dddddd;
  padding: 8px;
  color: #710000;
}

table td {
  border: 1px solid #dddddd;
  padding: 8px;
  color: #710000;
}

table thead:first-child {
  background-color: #ffb1b183; /* Fundo vermelho claro */
}
</style>