<template>
  <div>
    <p class="funcio">{{ title }}</p>
    <table class="tabela">
      <thead>
        <tr>
          <th>COLABORADORES ESCALADOS</th>
          <th>FUNÇÃO</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(funcao, nome) in funcionarios" :key="nome">
          <td>{{ nome }}</td>
          <td>{{ funcao }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';
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
      title: 'Funcionários escalados no dia',
      funcionarios: {},
    };
  },
  watch: {
    date: 'fetchData',
  },
  mounted() {
    this.fetchData(this.date);
  },
  methods: {
    async fetchData(date) {
        const formattedDate = this.formatDate(this.date);
      try {
        const response = await axios.get(`https://kitchen-back-production.up.railway.app/diario/${formattedDate}`);
        this.funcionarios = response.data.funcionarios || {};
      } catch (error) {
        console.error('Erro ao buscar dados:', error);
      }
    },
    formatDate(date) {
        // Adicione o T00:00:00 à data se ela não contiver
        return date.includes('T') ? date : `${date}T00:00:00`;
    },
  },
};
</script>

<style>
.tabela {
  width: 87%;
  border: 2px solid rgba(160, 160, 160, 0.554);
}

.tabela th, .tabela td {
  border: 1px solid rgba(160, 160, 160, 0.554);
  padding: 8px;
  text-align: center;
  font-family: 'Comic Sans MS';
}

.tabela th {
  background-color: #ff0000;
  color: white;
}

.funcio {
    font-family: 'Comic Sans MS';
    color: red;
    font-size: 22px;
    text-align: center;
    font-weight: bold;
    margin-bottom: 20px;
    margin-top: -40px;
}
</style>