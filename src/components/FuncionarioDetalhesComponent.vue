<template>
  <div class="container">
    <div class="component-nome">
      <H1>{{ funcionario.nome }}</H1>
    </div>
    <div>
      <VueDatePicker :model-value="selected" format="dd/MM/yyyy" locale="br" @update:model-value="dateSelected" range :placeholder="'Escolha um período'" class="custom-datepicker"/>
    </div>
    <div class="card-container2">
      <div class="card">
        <p>Total atendimentos</p>
        <p class="dadosCard">{{cardData.AtendimentosRealizados}} de {{cardData.totalAtendimentos}}</p>
      </div>
      <div class="card">
        <p>Média de avaliação</p>
        <p class="dadosCard">{{ !isNaN(avaliacaoData.mediaAvalicao) ? (avaliacaoData.mediaAvalicao / 2) : '0' }} ★</p>
      </div>
    </div>
    <div>
      <table-avaliacao></table-avaliacao>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import avaliacao from './AvaliacaoTableComponent.vue';

export default {
  name: 'funcionarioDetalhe',
  components: {
    'table-avaliacao': avaliacao,
  },
  data() {
    return {
      funcionario: {},
      cardData: {},
      avaliacaoData: {},
      idFunc: 0,
      dataInitial: '2023-09-01T00:00:00',
      dataEnd: '2023-09-30T00:00:00',
    };
  },
  created() {
    const funcionarioId = this.$route.params.id;
    this.fetchFuncionario(funcionarioId);
    this.idFunc = this.$route.params.idFunc;
    this.fetchCardData();
  },
  methods: {
    fetchMediaAvaliacao(idFunc, dataAvaliacaoIni, dataAvaliacaoFin) {
        axios.get(`https://kitchen-back-production.up.railway.app/funcionarios/media-notas`, {
          params: {
            idFuncionario: this.idFunc,
            dataAvaliacaoIni: this.dataInitial,
            dataAvaliacaoFin: this.dataEnd,
          }
        })
        .then(response => {
          if(response.data.length > 0){
          this.avaliacaoData = response.data[0];
          }else{
            alert('Sem dados de avaliação deste funcionário');
          }
        })
        .catch(error => {
          console.error('Erro ao buscar a média de avaliação: ', error);
        });
      },
    fetchFuncionario(id) {
      axios.get(`https://kitchen-back-production.up.railway.app/funcionarios/${id}`)
        .then(response => {
          this.funcionario = response.data;
          this.idFunc = this.funcionario.id; // Defina o idFunc com base nos dados recebidos
          this.fetchCardData();
          this.fetchMediaAvaliacao();
        })
        .catch(error => {
          console.error('Erro ao buscar informações do funcionário: ', error);
        });
    },

    fetchCardData(idFunc) {
      axios.get(`https://kitchen-back-production.up.railway.app/funcionarios/total-atendimentos/${this.idFunc}`, {
        params: {
          data1: this.dataInitial,
          data2: this.dataEnd,
        }
      })
      .then(response => {
        this.cardData = response.data;
      })
      .catch(error => {
        console.error('Erro ao realizar busca: ', error);
      });
    },

  dateSelected(modelData) {
      this.selected = modelData;
      if (this.selected) {
        const dataInitial = this.formatarData(this.selected[0]);
        const dataEnd = this.formatarData(this.selected[1]);
        console.log('Datas: ', this.selected);
        console.log('Data de inicio a ser usada:', dataInitial);
        console.log('Data de fim a ser usada:', dataEnd);
        this.dataInitial = dataInitial;
        this.dataEnd = dataEnd;
        this.fetchCardData(dataInitial, dataEnd);
        this.fetchMediaAvaliacao(this.idFunc, dataInitial, dataEnd);
      } else {
        console.log('Nenhuma data selecionada.');
      }
  },
  usarDataFormatada() {
      if (this.selected) {
        const dataInicio = this.formatarData(this.selected[0]);
        const dataFim = this.formatarData(this.selected[1]);
        console.log('Data de inicio a ser usada:', dataInicio);
        console.log('Data de fim a ser usada:', dataFim);
      } else {
        console.log('Nenhuma data selecionada.');
      }
    },
    formatarData(data) {
      const ano = data.getFullYear();
      const mes = (data.getMonth() + 1).toString().padStart(2, '0');
      const dia = data.getDate().toString().padStart(2, '0');
      const hora = data.getHours().toString().padStart(2, '0');
      const minutos = data.getMinutes().toString().padStart(2, '0');
      const segundos = data.getSeconds().toString().padStart(2, '0');

      return `${ano}-${mes}-${dia}T${hora}:${minutos}:${segundos}`;
    },
  },
};
</script>

<style>
.component-nome {
  width: 100%;
  color: #ff0000a4;
  font-family: 'Lilita One', sans-serif;
  text-align: left;
  margin-right: 950px;
  margin-left: 50px;
  font-size: 25px;
  margin-top: 20px;
}

.card-container2{
  display: flex;
  justify-content: left;
  margin: 10px;
  margin-left: 50px;
}

.card{
  margin-left: 10px;
}
</style>