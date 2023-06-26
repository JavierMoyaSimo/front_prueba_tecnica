<template>
  <div class="container-app">
    <div class="navigate">
      <router-link to="/">Home</router-link>
      <router-link to="/login">Login</router-link>
      <router-link to="/register">Register</router-link>
    </div>
    <!-- Etiqueta especial proporcionada por vue-router utilizada 
      para mostrar la vista correspondiente a la ruta actual. -->
    <div>
      <router-view></router-view>
    </div>
  </div>
</template>

<style scoped>
.container-app {
  display: flex;
  flex-direction: column;
  height: 100vh;
}

.navigate {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
}
</style>


<!-- 
<script setup>
//imports
import { ref, computed } from 'vue'
import Home from './components/Home.vue';
import Login from './components/Login.vue';
import Register from './components/Register.vue';


const routes = {
  '/': Home,
  '/login': Login,
  '/register': Register
}

const currentPath = ref(window.location.hash)

window.addEventListener('hashchange', () => {
  currentPath.value = window.location.hash
})

const currentView = computed(() => {
  return routes[currentPath.value.slice(1) || '/'] 
})
</script>




<template>
  <div class="container-app">
    <div class="navigate">
     <a href="#/">Home</a> 
  <a href="#/login">Login</a>
  <a href="#/register">Register</a>
  </div>
  <div>
    <component :is="currentView" />
  </div>
  </div>
  
  
  
 
</template>



<style scoped>
.container-app {
  display: flex;
  flex-direction: column;
  height: 100vh;
}
.navigate {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;  
}
</style>

 -->




