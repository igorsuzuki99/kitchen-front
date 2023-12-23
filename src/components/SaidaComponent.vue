<template>
    <div>
      <form class="form-container" @submit.prevent="submitForm" >
        <div class="form-group">
          <label for="quantidadeSaida">Quantidade saída:</label>
          <input type="text" id="quantidadeSaida" v-model="formData.quantidadeSaida" />
        </div>
  
        <div class="form-group">
          <label for="motivoSaida">Motivo saída:</label>
          <input type="text" id="motivoSaida" v-model="formData.motivoSaida" />
        </div>
  
        <div class="form-group">
          <label for="codEstoque">Código estoque:</label>
          <input type="text" id="codEstoque" v-model="formData.codEstoque" />
        </div>
  
        <div class="form-group">
          <label for="codPrato">Prato:</label>
          <input type="text" id="codPrato" v-model="formData.codPrato" />
        </div>
  
        <button class="round-button red-button" type="submit">Enviar</button>
      </form>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    name: 'saida',
    data() {
      return {
        formData: {
          quantidadeSaida: 0,
          motivoSaida: "",
          codEstoque: 0,
          codPrato: 0
        }
      };
    },
    methods: {
      submitForm() {
        // Converter os dados do formulário para JSON
        const jsonData = JSON.stringify(this.formData);
        console.log(jsonData);
        
        axios.post('https://kitchen-back-production.up.railway.app/estoque/saida', jsonData, {
                headers: {
                    'Content-Type': 'application/json'
                }
            })
                .then(response => {
                    console.log('Resposta do servidor:', response.data);
                    this.formSubmitted = true;
                    alert("Saída registrada");
                })
                .catch(error => {
                    console.error('Erro na solicitação POST:', error);
                    alert("Erro ao registrar saída");
                });
      }
    }
  };
  </script>
  
  <style>
  
  </style>
  