<script setup>
//IMPORTS
import { ref, computed } from 'vue';
import axios from 'axios';
import { useRouter } from 'vue-router';
//ENDPOINT
const endpoint = 'http://localhost:3001/login';

//USER-STATE
const user = ref({
  email: '',
  password: '',
});

//USER-ERROR-STATE
const userError = ref({
  emailError: '',
  passwordError: '',
  loginError: '',
});

//ROUTER
const router = useRouter();

//VALIDATION OF FORM(En script, hay que obtener el valor de la propiedad para que funcione)
const isFormInvalid = computed(() => {
  return (
    !user.value.email ||
    !user.value.password
  );
});

//LOGIN-FUNCTION
const loginUser = async () => {
  try {
    const response = await axios.post(endpoint, {
      email: user.value.email,
      password: user.value.password,
    });
    console.log(response);
    if (response.data.result === 'SUCCESS') {
      // Guardar la información del usuario en localStorage
      localStorage.setItem('user', JSON.stringify(response.data.user))
      console.log("ESTE ES EL DATO A TRATAR",response.data.user)
      loginCorrectly.value = !loginCorrectly.value
      userError.value.loginError = 'CONECTADO CON ÉXITO';
      document.getElementById('loginerror').textContent = userError.value.loginError;
      setTimeout(() => {
        router.push('/userView');

      }, 1000);

    } else {
      userError.value.loginError = 'Email o contraseña incorrectos';
      document.getElementById('loginerror').textContent = userError.value.loginError;
    }
  } catch (error) {
    console.error('Login fallido', error);

    userError.value.loginError = 'Email o contraseña incorrectos';
    document.getElementById('loginerror').textContent = userError.value.loginError;
  }
};

// SEE PASSWORD / NO SEE PASSWORD
// const passwordShown = ref(false);
// const togglePassword = () => {
//   passwordShown.value = !passwordShown.value;
// };

//LOGIN-SUCCESSFUL
let loginCorrectly = ref(false);

</script>

<template>
  <form @submit.prevent="loginUser" class="form-div">
    <h2>INICIAR SESIÓN </h2>
    <!-- EMAIL -->
    <div class="form-div">
      <input v-model.trim="user.email" type="email" class="form-control" placeholder="Email" required>
      <div class="error-message">{{ userError.emailError }}</div>
    </div>
    <!-- PASSWORD -->
    <div class="form-div">
      <input v-model.trim="user.password" type= 'password' class="form-control"
        placeholder="Contraseña" required>
      <div class="error-message">{{ userError.passwordError }}</div>
      <!-- <span @click="togglePassword" class="">
        <i :class="passwordShown ? 'see-password' : 'no-see-password'"></i>
      </span> -->
    </div>
    <!-- BUTTON-REGISTER -->
    <button :disabled="isFormInvalid" class="">Conectarse</button>
    <div id="loginerror" :class="loginCorrectly ? 'successful-message' : 'error-message'">{{ userError.loginError }}
    </div>
  </form>
</template>


<style scoped>
.form-div {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin-top: 1em;
}

button {
  margin-top: 1em;
}

.error-message {
  color: red;
  font-size: small;
}

.successful-message {
  color: green;
  font-size: small;
}
</style>