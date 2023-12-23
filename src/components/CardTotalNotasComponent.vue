<template>
    <div class="card">
      <p>Total notas</p>
      <p class="dadosCard">{{ totalNota }}</p>
    </div>
  </template>
  
  <script>
  export default {
    props: ['totalNota']
  }
  </script>