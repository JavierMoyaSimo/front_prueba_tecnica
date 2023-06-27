<script setup>
//IMPORTS
import { ref, computed } from 'vue';
import axios from 'axios';
import { useRouter } from 'vue-router';
//ENDPOINT
const endpoint = 'http://localhost:3001/adduser';

//USER-STATE
const user = ref({
    name: '',
    phone: '',
    email: '',
    password: '',
});

//USER-ERROR-STATE
const userError = ref({
    nameError: '',
    phoneError: '',
    emailError: '',
    passwordError: '',
    registerError: '',
});

//ROUTER
const router = useRouter();

//VALIDATION OF FORM(En script, hay que obtener el valor de la propiedad para que funcione)
const isFormInvalid = computed(() => {
    return (
        !user.value.name ||
        !user.value.phone ||
        !user.value.email ||
        !user.value.password
    );
});

//REGISTER-FUNCTION
const registerUser = async () => {
    try {
        const response = await axios.post(endpoint, {
            name: user.value.name,
            phone: user.value.phone,
            email: user.value.email,
            password: user.value.password,
        });

        if (response.data.result === 'SUCCESS') {
            registerCorrectly.value = !registerCorrectly.value
            userError.value.registerError = 'REGISTRADO CON ÉXITO';
            document.getElementById('regerror').textContent = userError.value.registerError;
            setTimeout(() => {
                router.push('/login');

            }, 1000);

        } else {
            userError.value.registerError = 'Registro fallido, revise todos los campos';
            document.getElementById('regerror').textContent = userError.value.registerError;
        }
    } catch (error) {
        console.error('Registro fallido', error);

        userError.value.registerError = 'Registro fallido, revise todos los campos';
        document.getElementById('regerror').textContent = userError.value.registerError;
    }
};

// SEE PASSWORD / NO SEE PASSWORD
// const passwordShown = ref(false);
// const togglePassword = () => {
//     passwordShown.value = !passwordShown.value;
// };

//REGISTER-SUCCESSFUL
let registerCorrectly = ref(false);



</script>

<template>
    <form @submit.prevent="registerUser" class="form-div">
        <h2>INSCRÍBETE </h2>
        <!-- NAME -->
        <div class="form-div">
            <input v-model.trim="user.name" type="text" class="form-control" placeholder="Nombre" required>
            <div class="error-message">{{ userError.nameError }}</div>
        </div>
        <!-- PHONE -->
        <div class="form-div">
            <input v-model.trim="user.phone" type="text" class="form-control" placeholder="Teléfono" required>
            <div class="error-message">{{ userError.phoneError }}</div>
        </div>
        <!-- EMAIL -->
        <div class="form-div">
            <input v-model.trim="user.email" type="email" class="form-control" placeholder="Email" required>
            <div class="error-message">{{ userError.emailError }}</div>
        </div>
        <!-- PASSWORD -->
        <div class="form-div">
            <input v-model.trim="user.password" type='password' class="form-control" placeholder="Contraseña" required>
            <div class="error-message">{{ userError.passwordError }}</div>
            <!-- <span @click="togglePassword" class="">
                <i :class="passwordShown ? 'see-password' : 'no-see-password'"></i>
            </span> -->
        </div>
        <!-- BUTTON-REGISTER -->
        <button :disabled="isFormInvalid" class="">Registrarme</button>
        <div id="regerror" :class="registerCorrectly ? 'successful-message' : 'error-message'">{{ userError.registerError }}
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





