<template>
  <header class="navbar">
    <div class="navbar-left">
      <span class="navbar-text">CLOUD KITCHEN</span>
    </div>
    <div class="navbar-right">
      <i class="fas fa-bell bell-icon" @click="handleBellClick" ref="bellIcon"></i>
      <i class="fas fa-circle-user user-icon" @click="handleUserClick"></i>
    </div>
  </header>

  <!-- Dropdown -->
  <div v-if="showDropdown" class="dropdown" :style="dropdownStyle">
    <ul>
      <li><a href="#">Item 1</a></li>
      <li><a href="#">Item 2</a></li>
      <li><a href="#">Item 3</a></li>
    </ul>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';

const showDropdown = ref(false);
const bellIcon = ref(null);
const dropdownStyle = ref({
  top: '0px',
  left: '0px',
});

const handleBellClick = () => {
  showDropdown.value = !showDropdown.value; // Alternar a visibilidade do dropdown
}

const handleUserClick = () => {
  // Lógica para lidar com o clique no ícone do usuário
}

</script>

  

<style scoped>
.dropdown {
  position: absolute;
  top: 50px; /* Ajuste a posição conforme necessário */
  right: 10px; /* Ajuste a posição conforme necessário */
  background-color: #fff;
  border: 1px solid #ccc;
  padding: 10px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.dropdown ul {
  list-style: none;
  padding: 0;
}

.dropdown li {
  margin: 5px 0;
}

.dropdown a {
  text-decoration: none;
  color: #333;
  font-size: 16px;
}

.dropdown a:hover {
  color: #007bff;
}
.navbar {
  background: #ff0000; /* Cor de fundo da navbar */
  color: white; /* Cor do texto na navbar */
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 20px; /* Espaçamento interno da navbar */
  position: fixed; /* Fixa a barra de navegação no topo */
  top: 0; /* Define a posição no topo da página */
  left: 0; /* Define a posição no canto esquerdo */
  right: 0; /* Define a largura total */
  z-index: 999; /* Define uma ordem de sobreposição alta */
}

.navbar-text {
  font-family: 'Lilita One', sans-serif;
  font-size: 24px;
}

/* Estilos para o texto "Cloud Kitchen" no canto esquerdo */
.navbar-left {
  font-weight: bold;
}

/* Estilos para a seção do canto direito com os ícones */
.navbar-right {
  font-size: 24px;
  display: flex;
  align-items: center;
}

/* Estilo para o ícone de sino */
.bell-icon {
  margin-right: 40px; 
  color: #ffffff; 
}

/* Estilo para o ícone de usuário */
.user-icon {
  margin-right: 20px;
  color: #ffffff;
}

nav {
  background: #ff0000a4;
  position: fixed;
  left: 0;
  top: 0;
  height: 100%;
  width: 100px;
  text-align: left;
  display: flex;
  flex-direction: column;
  margin-top: 10px;
  justify-content: center;
}

nav ul {
  padding-top: 15rem;
  display: flex;
  justify-content: space-between;
  flex-direction: column;
}
nav li {
  text-align: center;
}

.sidebar-text {
  font-size: 18px;
}
.sidebar-link {
  text-decoration: none;
  color: white;
  font-family: 'Lilita One', sans-serif;
  display: flex;
  flex-direction: column; /* Alteração para exibir ícone acima do texto */
  align-items: center;
  justify-content: center;
  height: 18%;
  margin-bottom: 1px;
}

.sidebar-link i {
  margin-bottom: 30px; /* Espaçamento entre o ícone e o texto */
}
</style>