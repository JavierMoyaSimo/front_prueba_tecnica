<script setup>
import { ref, onMounted, computed } from 'vue'
import { useRouter } from 'vue-router';
import axios from 'axios';

const user = ref(null);
const dataToUpdate = ref(null);

//USER-ERROR-STATE
const userError = ref({
    emailError: '',
    phoneError: '',
    passwordError: '',
    updateError: '',
});

//ENDPOINT
const endpoint = 'http://localhost:3001/modifyuser/';

onMounted(() => {
    const userData = localStorage.getItem('user');
    console.log("ESTO ES USERDATA DEL LOCALSTORAGE", userData)
    if (userData) {
        user.value = JSON.parse(userData);
        console.log("Este es el user", user.value);
        dataToUpdate.value = { ...user.value };
        console.log("ESTO ES DATATOUPDATE", dataToUpdate);
        console.log("GUENO", dataToUpdate.value.phone)
    }
})

//ROUTER
const router = useRouter();

// VALIDATION OF FORM
const isFormInvalid = computed(() => {
    return (
        !dataToUpdate.value ||
        !dataToUpdate.value.name ||
        !dataToUpdate.value.email ||
        !dataToUpdate.value.password ||
        !dataToUpdate.value.phone
    );
});

//UPDATE-USER-FUNCTION
const updateUser = async () => {
    try {
        console.log("DATOS RECIBIDOS", dataToUpdate)
        const response = await axios.patch(endpoint + user.value.email, {
            name: dataToUpdate.value.name,
            phone: dataToUpdate.value.phone,
            password: dataToUpdate.value.password,
        }, {
            headers: { Authorization: `Bearer ${user.value.jwt}` },
        });
        console.log("EEEEEEEEEEESTOOOOOO", response.data.message)
        if (response.data.message === "User not found") {
            userError.value.updateError = 'Error en la actualización, revisa los campos';
            document.getElementById('updateError').textContent = userError.value.updateError;
            return;
        }
        console.log("RESPUPDATE", response);
        updateCorrectly.value = !updateCorrectly.value
        userError.value.updateError = 'ACTUALIZADO CON ÉXITO';
        document.getElementById('updateError').textContent = userError.value.updateError;
        setTimeout(() => {
            localStorage.removeItem('user');
            router.push('/login');
        }, 1000);

    } catch (error) {
        console.error('Actualización fallida', error);
        userError.value.updateError = 'Error en la actualización';
        document.getElementById('updateError').textContent = userError.value.updateError;
    }
}

//UPDATE-SUCCESSFUL
let updateCorrectly = ref(false);
</script>

<template>
    <div class="userView-div">
        <!-- Rol user -->
        <div v-if="user && user.rol === 'user'">
            <h2>Bienvenido, {{ user && user.name }}!</h2>
            <div class="row-div">
                <p class="label">Nombre:</p>
                <p>{{ user && user.name }}</p>
            </div>
            <div class="row-div">
                <p class="label">Email:</p>
                <p>{{ user && user.email }}</p>
            </div>
            <div class="row-div">
                <p class="label">Teléfono:</p>
                <p>{{ user && user.phone }}</p>
            </div>

            <form @submit.prevent="updateUser" class="userView-div">
                <h2>ACTUALIZAR DATOS </h2>
                <!-- NAME -->
                <div class="userView-div">
                    <input v-model.trim="dataToUpdate.name" type="text" class="form-control"
                        :placeholder="user && user.name" required>
                    <div class="error-message">{{ userError.emailError }}</div>
                </div>
                <!-- PHONE -->
                <div class="userView-div">
                    <input v-model.trim="dataToUpdate.phone" type="text" class="form-control"
                        :placeholder="user && user.phone" required>
                    <div class="error-message">{{ userError.phoneError }}</div>
                </div>
                <!-- PASSWORD -->
                <div class="userView-div">
                    <input v-model.trim="dataToUpdate.password" type='password' class="form-control"
                        placeholder="Contraseña" required>
                    <div class="error-message">{{ userError.passwordError }}</div>
                </div>
                <!-- BUTTON-UPDATE-USER -->
                <button :disabled="isFormInvalid" class="">Actualizar datos</button>
                <div id="updateError" :class="updateCorrectly ? 'successful-message' : 'error-message'">{{
                    userError.updateError }}</div>
            </form>

        </div>

        <!-- Rol admin -->
        <div v-if="user && user.rol === 'admin'">
            <h2>Bienvenido, Administrador!</h2>
        </div>
    </div>
</template>

<style scoped>
.userView-div {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    margin-top: 1em;
}

.row-div {
    display: flex;
    justify-content: space-between;
}

.label {
    font-weight: bold;
}

h2 {
    color: green;
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
