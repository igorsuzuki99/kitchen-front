<template>
    <div>
        <form class="form-container" @submit.prevent="submitForm">
            <div class="form-group">
                <label for="nomeAlerta">Nome do Alerta:</label>
                <input type="text" id="nomeAlerta" v-model="formData.nomeAlerta" />
            </div>

            <div class="form-group">
                <label for="descricao">Descrição:</label>
                <input type="text" id="descricao" v-model="formData.descricao" />
            </div>

            <div class="form-group">
                <label for="entidade">Entidade:</label>
                <select id="entidade" v-model="formData.entidade">
                    <option value="ESTOQUE">ESTOQUE</option>
                    <!-- <option value="OUTRA_ENTIDADE">OUTRA_ENTIDADE</option> -->
                </select>
            </div>

            <div class="form-group">
                <label for="condicaoDisparo">Condição de Disparo:</label>
                <select id="condicaoDisparo" v-model="formData.condicaoDisparo">
                    <option value="QUANTIDADE_EM_ESTOQUE">QUANTIDADE_EM_ESTOQUE</option>
                    <option value="VALIDADE">VALIDADE</option>
                </select>
            </div>

            <div class="form-group">
                <label for="valorParametro">Valor do Parâmetro:</label>
                <input type="text" id="valorParametro" v-model="formData.valorParametro" />
            </div>

            <div class="form-group">
                <label for="acao">Ação:</label>
                <select id="acao" v-model="formData.acao">
                    <option value="ENVIAR_EMAIL">ENVIAR_EMAIL</option>
                    <!-- <option value="OUTRA_ACAO">OUTRA_ACAO</option> -->
                </select>
            </div>

            <div class="form-group">
                <label for="destinatarios">Destinatários:</label>
                <input type="text" id="destinatarios" v-model="formData.destinatarios" />
            </div>

            <div class="form-group text-right">
                <button class="round-button red-button" type="submit">Enviar</button>
            </div>
        </form>
    </div>
</template>
  
<script>
import axios from 'axios';

export default {
    name: 'alerta',
    data() {
        return {
            formData: {
                nomeAlerta: "",
                descricao: "",
                entidade: "",
                condicaoDisparo: "",
                valorParametro: "",
                acao: "",
                destinatarios: ""
            },
        };
    },
    methods: {
        submitForm() {
            const jsonData = JSON.stringify(this.formData);
            console.log(jsonData);

            axios.post('https://kitchen-back-production.up.railway.app/alerta', jsonData, {
                headers: {
                    'Content-Type': 'application/json'
                }
            })
                .then(response => {
                    console.log('Resposta do servidor:', response.data);
                    this.formSubmitted = true;
                    alert("Alerta criado");
                })
                .catch(error => {
                    console.error('Erro na solicitação POST:', error);
                    alert("Erro ao criar alerta");
                });
        }
    }
};
</script>

<style>
.form-group select {
    width: 25%;
    /* Defina a largura máxima para que todos os campos tenham a mesma largura */
    padding: 5px;
    border: 1px solid #ccc;
    border-radius: 4px;
    margin-top: 5px;
    /* Alinhe os campos na parte superior */
}
</style>