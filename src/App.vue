<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import NavBarComponent from './components/NavBarComponent.vue';
</script>

<template>
  <header>
    <link
    rel="stylesheet"
    href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css"/>
    <NavBarComponent/>
    <div class="wrapper">

      <nav>

          <RouterLink to="/dash" class="sidebar-link">
            <i class="fa-solid fa-chart-bar fa-2xl"></i>
            <span class="sidebar-text">Dashboard</span>
          </RouterLink>

          <RouterLink to="/painel" class="sidebar-link">
            <i class="fa-brands fa-microsoft fa-2xl"></i>
            <span class="sidebar-text">Painel</span>
          </RouterLink>

          <RouterLink to="/gestao" class="sidebar-link">
            <i class="fa-regular fa-user fa-2xl"></i>
            <span class="sidebar-text">Gestão</span>
          </RouterLink>
          
          <RouterLink to="/operacoes" class="sidebar-link">
            <i class="fa-brands fa-whmcs fa-2xl"></i>
            <span class="sidebar-text">Operações</span>
          </RouterLink>
      </nav>
    </div>
  </header>

  <RouterView />
</template>

<style scoped>
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
