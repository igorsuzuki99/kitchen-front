<template>
  

        <div class="search-input">
          <div class="legend">
            <div class="legend-item">
              <div class="legend-color estoque-baixo"></div>
              <p>Quantidade próxima ou abaixo do mínimo</p>
            </div>
          </div>
            <input placeholder="Pesquisar produto" class="input-search" v-model="searchTerm">
            <!-- <button class="search-button"><i class="fa-regular fa-paper-plane"></i></button> -->
        </div>

        <table class="table-data">
            <thead>
            <tr>
                <th>Cód</th>
                <th>Produto</th>
                <th>Unidade de Medida</th>
                <th>Categoria</th>
                <th>Quantidade</th>
                <!-- <th>Capacidade</th> -->
                <th>Estoque Mínimo</th>
            </tr>
            </thead>
            <tbody>
              <tr v-for="item in filteredEstoque" :key="item.codIngrediente" :class="{ 'estoque-baixo': isEstoqueBaixo(item) }" @click="redirecionarParaFornecedores(item.codIngrediente)">
                    <td>{{ item.codIngrediente }}</td>
                    <td class="nome-ingrediente">{{ item.nomeIngrediente }}</td>
                    <td>{{ item.unidadeMedida }}</td>
                    <td>{{ item.categoriaIngrediente }}</td>
                    <td>{{ item.quantidadeTotal }}</td>
                    <!-- <td>{{ item.capacidade }}</td> -->
                    <td>{{ item.estoqueMinimo }}</td>
                </tr>
            </tbody>
        </table>
</template>

<script>
import axios from 'axios';
export default {
    name: 'estoque',
    data() {
      return {
        estoqueData: [],
        estoqueBaixoData: [],
        searchTerm: '',
      };
    },
    computed: {
      filteredEstoque() {
        const termo = this.searchTerm.toLowerCase();
        return this.estoqueData.filter(item => item.nomeIngrediente.toLowerCase().includes(termo));
      }
    },

    mounted() {
      this.fetchData();
    },
    methods: {
      fetchData() {
        axios.get('https://kitchen-back-production.up.railway.app/ingrediente/listar')
        .then(response => {
          this.estoqueData = response.data;
        })
        .catch(error => {
          console.error('Erro ao realizar busca no estoque: ', error);
        });

        axios.get('https://kitchen-back-production.up.railway.app/ingrediente/estoque-baixo')
        .then(response => {
          this.estoqueBaixoData = response.data;
        })
        .catch(error => {
          console.error('Erro ao buscar estoque baixo: ', error);
        });
      },
      redirecionarParaFornecedores(codIngrediente) {
        this.$router.push({ name: 'fornecedores-ingrediente', params: { codIngrediente } });
      },
      isEstoqueBaixo(item) {
      // Verifica se o item está na lista de estoque baixo
      return this.estoqueBaixoData.some(estoqueBaixoItem => estoqueBaixoItem.codIngrediente === item.codIngrediente);
    },

    }

}
</script>

<style>
.legend {
  text-align: left;
}

.legend-item {
  display: inline-flex;
  align-items: center;
  margin-right: 450px;
}

.legend-color {
  width: 20px;
  height: 20px;
  margin-right: 5px;
}

.estoque-baixo {
  background-color: #ffb1b14d; /* Cor de fundo para destacar as linhas de estoque baixo */
}

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
    table td.nome-ingrediente:hover {
      cursor: pointer;
    }
</style>