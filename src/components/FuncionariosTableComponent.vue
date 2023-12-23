<template>
    <div class="search-input">
        <input placeholder="Pesquisar funcionário" class="input-search" v-model="searchTerm">
    </div>

    <table class="table-data">
        <thead>
        <tr>
            <th>ID</th>
            <th>Nome funcionário</th>
            <th>Função</th>
            <th>Dia de folga</th>
        </tr>
        </thead>
        <tbody>
            <tr v-for="item in filteredFuncionario" :key="item.cod">
                <td>{{ item.id }}</td>
                <td class="nome-prato"><span @click="redirecionarParaDetalhes(item.id)">{{ item.nome }}</span></td>
                <td>{{ item.funcao }}</td>
                <td>{{ item.diaFolga }}</td>
            </tr>
        </tbody>
    </table>
</template>

<script>
import axios from 'axios';
export default {
name: 'funcionario',
data() {
  return {
    funcionarioData: [],
    searchTerm: '',
  };
},
computed: {
  filteredFuncionario() {
    const termo = this.searchTerm.toLowerCase();
    return this.funcionarioData.filter(item => item.nome.toLowerCase().includes(termo));
  }
},

mounted() {
  this.fetchData();
},
methods: {
  fetchData() {
    axios.get('https://kitchen-back-production.up.railway.app/funcionarios')
    .then(response => {
      this.funcionarioData = response.data;
    })
    .catch(error => {
      console.error('Erro ao realizar busca dos funcionários: ', error);
    });
  },
  redirecionarParaDetalhes(id) {
      // Emita um evento personalizado com o ID do funcionário para o componente pai
      this.$emit('redirecionar-para-detalhes', id);
    },
}

}
</script>

<style>
table td.nome-prato:hover {
  cursor: pointer;
}
</style>