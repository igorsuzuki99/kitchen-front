<template>
    <div class="card">
      <p>Receita Total</p>
      <p class="dadosCard">R$ {{ receitaTotal }}</p>
    </div>
  </template>
  
  <script>
  export default {
    props: ['receitaTotal']
  }
  </script>