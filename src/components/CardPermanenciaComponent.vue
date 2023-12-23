<template>
    <div class="card">
      <p>Permanência média</p>
      <p class="dadosCard">{{ permancencia }}</p>
    </div>
  </template>
  
  <script>
  export default {
    props: ['permancencia']
  }
  </script> 