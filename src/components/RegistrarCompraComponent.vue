<template>
    <div>
      <form @submit.prevent="submitForm" class="form-container">
        <div class="form-group">
            <label for="valorCompra">Valor da Compra:</label>
            <input type="text" id="valorCompra" v-model="formData.valorCompra" />
        </div>
  
        <div class="form-group">
          <label for="idFornecedor">ID do Fornecedor:</label>
          <input type="text" id="idFornecedor" v-model="formData.idFornecedor" />
        </div>
            <br>
        <div class="form-group">
          <label for="itens">ITENS:</label>
          <div class="items-group" v-for="(item, index) in formData.itens" :key="index">
            <div class="item-group">
              <label :for="'descricaoItem-' + index">Descrição do Item:</label>
              <input :id="'descricaoItem-' + index" v-model="item.descricaoItem" />
            </div>
  
            <div class="item-group">
              <label :for="'codIngrediente-' + index">Código do Ingrediente:</label>
              <input :id="'codIngrediente-' + index" v-model="item.codIngrediente" />
            </div>
          </div>
          <button @click="addItem" class="round-button red-button" type="button">Adicionar Item</button>
        </div>
        <button type="submit" class="round-button red-button">Enviar</button>
  
        
      </form>
    </div>
  </template>
  
  <script>
  import axios from 'axios';

  export default {
    data() {
      return {
        formData: {
          valorCompra: 0,
          idFornecedor: 0,
          itens: [
            {
              descricaoItem: " ",
              codIngrediente: 0
            }
          ]
        }
      };
    },
    methods: {
      submitForm() {
        // Exibir a estrutura JSON no console
        const jsonData = JSON.stringify(this.formData, null, 2);
        console.log(jsonData);

        axios.post('https://kitchen-back-production.up.railway.app/compra', jsonData, {
                headers: {
                    'Content-Type': 'application/json'
                }
            })
                .then(response => {
                    console.log('Resposta do servidor:', response.data);
                    this.formSubmitted = true;
                    alert("Compra registrada");
                })
                .catch(error => {
                    console.error('Erro na solicitação POST:', error);
                    alert("Erro ao registrar compra");
                });
      },

      addItem() {
        this.formData.itens.push({
          descricaoItem: " ",
          codIngrediente: 0
        });
      }
    }
  };
  </script>
  
  <style>
.form-group label {
  margin-right: 20px;
  display: inline-block;
  width: 200px;
}

.form-group input,
.item-group input {
  width: 25%; /* Defina a largura máxima para que todos os campos tenham a mesma largura */
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 4px;
  margin-top: 5px; /* Alinhe os campos na parte superior */
}

.items-group {
    margin-top: 15px;
}

.round-button {
  border-radius: 10px; /* Define a borda arredondada */
  background-color: red; /* Define a cor de fundo vermelha */
  color: white; /* Define a cor do texto como branco */
  padding: 8px 20px; /* Ajuste o preenchimento conforme necessário */
  cursor: pointer;
  border: none;
  margin-top: 20px;
  margin-right: 5px;
}

.round-button:hover {
  background-color: darkred; /* Define a cor de fundo quando o botão é destacado */
}
  </style>
  