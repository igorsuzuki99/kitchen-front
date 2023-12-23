<template>
    <div class="container">
      <div class="component-title">
        <h1>PESSOAL</h1>
      </div>
      <div>
        <VueDatePicker :model-value="selected" format="dd/MM/yyyy" locale="br" @update:model-value="dateSelected" range :placeholder="'Escolha um período'" class="custom-datepicker" />
      </div>
      <div class="container-chart">
        <chart-atendimento ref="chartAtendimento"></chart-atendimento>
        <chart-avaliacao :dataInicio="dataInitial" :dataFim="dataEnd" ref="barChart"></chart-avaliacao>
      </div>
      <div>
        <table-funcionario @redirecionar-para-detalhes="redirecionarParaDetalhes"></table-funcionario>
      </div>
  
    </div>
</template>

<script>
import axios from 'axios';
import funcionario from './FuncionariosTableComponent.vue';
import VueDatePicker from '@vuepic/vue-datepicker';
import '@vuepic/vue-datepicker/dist/main.css';
import ChartAtendimentosComponent from './ChartAtendimentosComponent.vue';
import BarChartAtendimentosComponent from './BarChartAtendimentosComponent.vue';

export default {
  name: 'gestao',
  components: {
    'table-funcionario': funcionario,
    'chart-atendimento': ChartAtendimentosComponent,
    'chart-avaliacao': BarChartAtendimentosComponent,
  },
  methods: {
    updateChartWithDates() {
      this.$refs.chartAtendimento.fetchDataAndCreateChart(this.dataInitial, this.dataEnd);
      this.$refs.barChart.fetchDataAndCreateBarChart(this.dataInitial, this.dataEnd);
    },
    redirecionarParaDetalhes(id) {
      this.$router.push({ name: 'funcionario', params: { id: id } });
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
  data() {
    return {
      funcionarios: [],
      selectedDate: null,
      selected: null,
      dataInitial: null,
      dataEnd: null,
    };
  },
  watch: {
    dataInitial: 'updateChartWithDates',
    dataEnd: 'updateChartWithDates',
  },
  fetchData() {
    axios.get('https://kitchen-back-production.up.railway.app/funcionarios')
    .then(response => {
      this.funcionarios = response.data;
      console.log(this.funcionarios)
    })
    .catch(error => {
      console.error('Erro ao realizar busca: ', error);
    });
  },
};
</script>

<style>
.container-chart{
  display: flex;
  margin-top: 2%;
}

.custom-datepicker{
  width: 400px;
  margin-left: 68%
}
</style>