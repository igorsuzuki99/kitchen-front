<template>
    <div>
      <form class="form-container" @submit.prevent="submitForm" >
        <div class="form-group">
          <label for="pesoProduto">Peso do Produto:</label>
          <input type="text" id="pesoProduto" v-model="formData.pesoProduto" />
        </div>
  
        <div class="form-group">
          <label for="quantidadeProduto">Quantidade do Produto:</label>
          <input type="text" id="quantidadeProduto" v-model="formData.quantidadeProduto" />
        </div>
  
        <div class="form-group">
          <label for="dataValidade">Data de Validade:</label>
          <input type="date" id="dataValidade" v-model="formData.dataValidade" />
        </div>
  
        <div class="form-group">
          <label for="codItemCompra">Código do Item de Compra:</label>
          <input type="text" id="codItemCompra" v-model="formData.codItemCompra" />
        </div>
  
        <button class="round-button red-button" type="submit">Enviar</button>
      </form>
    </div>
  </template>
  
  <script>
  import axios from 'axios';

  export default {
    name: 'entrada',
    data() {
      return {
        formData: {
          pesoProduto: 0,
          quantidadeProduto: 0,
          dataValidade: "",
          codItemCompra: 0
        }
      };
    },
    methods: {
      submitForm() {
        const jsonData = JSON.stringify(this.formData);
        console.log(jsonData);

        axios.post('https://kitchen-back-production.up.railway.app/estoque/entrada', jsonData, {
                headers: {
                    'Content-Type': 'application/json'
                }
            })
                .then(response => {
                    console.log('Resposta do servidor:', response.data);
                    this.formSubmitted = true;
                    alert("Entrada registrada");
                })
                .catch(error => {
                    console.error('Erro na solicitação POST:', error);
                    alert("Erro ao registrar entrada");
                });
      }
    }
  };
  </script>
  
  <style>
  
  </style>
  