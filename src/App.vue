<template>
  <div class="container-app">
    <div class="navigate">
      <router-link to="/register">Register</router-link>
      <router-link to="/">Home</router-link>
      <router-link to="/login">Login</router-link>

    </div>
    <!-- Etiqueta  proporcionada por vue-router utilizada 
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
  justify-content: space-evenly;
  align-items: flex-start;
}
</style>



