<template>
    <table class="table-data">
        <thead>
        <tr>
            <th>Cód Nota</th>
            <th>Nota atendimento</th>
            <th>Comentário</th>
            <th>Data da avaliação</th>
        </tr>
        </thead>
        <tbody>
            <tr v-for="item in filteredFuncionario" :key="item.codNota">
                <td>{{ item.codNota }}</td>
                <td>{{ item.notaAtendimento/2 }}</td>
                <td>{{ item.comentario }}</td>
                <td>{{ formatDateTime(item.dataAvaliacao) }}</td>
            </tr>
        </tbody>
    </table>
</template>

<script>
import axios from 'axios';
export default {
name: 'avaliacao',
data() {
  return {
    funcionarioData: [],
    searchTerm: '',
  };
},
created() {
  const funcionarioId = this.$route.params.id;
  this.fetchData(funcionarioId);
},
computed: {
  filteredFuncionario() {
    const termo = this.searchTerm.toLowerCase();
    return this.funcionarioData.filter(item => item.comentario.toLowerCase().includes(termo));
  }
},

mounted() {
  this.fetchData();
},
methods: {
  fetchData(id) {
    axios.get(`https://kitchen-back-production.up.railway.app/funcionarios/avaliacao/${id}`)
    .then(response => {
      this.funcionarioData = response.data;
    })
    .catch(error => {
      console.error('Erro ao realizar busca dos funcionários: ', error);
    });
  },
  filterDataByDate() {
    if (!this.selectedDate) {
      // Se a data não estiver selecionada, exiba todos os dados
      return this.notasData;
    } else {
      // Se uma data estiver selecionada, filtre os dados com base nessa data
      const selectedDate = new Date(this.selectedDate).toISOString().split('T')[0];
      return this.notasData.filter((item) => {
        const itemDate = item.abertura.split('T')[0]; // Supondo que a data está no formato ISO
        return itemDate === selectedDate;
      });
    }
  },
  formatDateTime(dateTime) {
    if (!dateTime) return '';

    const date = new Date(dateTime);
    const options = {
      year: 'numeric',
      month: '2-digit',
      day: '2-digit',
      hour: '2-digit',
      minute: '2-digit',
    };

    return date.toLocaleString('pt-BR', options);
  },
}

}
</script>