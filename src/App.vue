<!-- LOGIC -->
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



<!-- TEMPLATE -->
<template>
  <div class="container-app">
    <div class="navigate">
     <a href="#/">Home</a> 
  <a href="#/Login">Login</a>
  <a href="#/Register">Register</a>
  </div>
  <div>
    <component :is="currentView" />
  </div>
  </div>
  
  
  
 
</template>


<!-- STYLES -->
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






