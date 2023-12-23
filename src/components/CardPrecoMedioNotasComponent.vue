<template>
    <div class="card">
      <p>Média das notas</p>
      <p class="dadosCard">R$ {{ precoMedioNota }}</p>
    </div>
  </template>
  
  <script>
  export default {
    props: ['precoMedioNota']
  }
  </script>