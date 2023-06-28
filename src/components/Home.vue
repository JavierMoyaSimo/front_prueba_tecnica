<script setup>
import { useRouter } from 'vue-router';


const router = useRouter();

const goRegister = () => {
  router.push('/register');
};

const goLogin = () => {
  router.push('/login');
};
</script>

<template>
  <div class="home-div">
    <h1>HOME</h1>
    <div class="buttons-div">
      <div class="register-login-div">
        <p>Regístrate</p>
        <button @click="goRegister">REGISTER</button>
      </div>
      <div class="register-login-div">
        <p>Inicia sesión</p>
        <button @click="goLogin">LOGIN</button>
      </div>

    </div>
  </div>
</template>

<style scoped>
.home-div {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

.buttons-div {
  display: flex;
  justify-content: space-evenly;
  align-items: center;
  width: 100%;
}

.register-login-div {
  display: flex;
  text-align: center;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  border-radius: 10px;
  border: 1px solid gray;
  background-color: rgba(37, 132, 167, 0.575);
  padding: 1em;
  width: 35%;
}

p {
  font-weight: bold;
  font-family: Georgia, 'Times New Roman', Times, serif;
}
</style>





