<script setup>
import { ref, onMounted } from 'vue'

const user = ref(null);

onMounted(() => {
    const userData = localStorage.getItem('user');
    console.log("ESTO ES USERDATA DEL LOCALSTORAGE", userData)
    if (userData) {
        user.value = JSON.parse(userData);
        console.log("Este es el user", user.value);
    }
})

let dataToUpdate = ref(user.value)
console.log("ESTO ES DATATOUPDATE", dataToUpdate);

//VALIDATION OF FORM(En script, hay que obtener el valor de la propiedad para que funcione)
// const isFormInvalid = computed(() => {
//     return (
//         !dataToUpdate.value.email ||
//         !dataToUpdate.value.password ||
//         !dataToUpdate.value.phone
//     );
// });

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

            <form @submit.prevent="" class="userView-div">
                <h2>ACTUALIZAR DATOS </h2>
                <!-- NAME -->
                <div class="userView-div">
                    <input :v-model.trim="dataToUpdate && dataToUpdate.name" type="text" class="form-control"
                        :placeholder="user && user.name" required>
                    <!-- <div class="error-message">{{ userError.emailError }}</div> -->
                </div>
                <!-- PHONE -->
                <div class="userView-div">
                    <input :v-model.trim="dataToUpdate && dataToUpdate.phone" type="text" class="form-control"
                    :placeholder="user && user.phone" required>
                    <!-- <div class="error-message">{{ userError.phoneError }}</div> -->
                </div>
                <!-- PASSWORD -->
                <div class="userView-div">
                    <input :v-model.trim="dataToUpdate && dataToUpdate.password" type='password' class="form-control"
                        placeholder="Contraseña" required>
                    <!-- <div class="error-message">{{ userError.passwordError }}</div> -->

                </div>
                <!-- BUTTON-REGISTER -->
                <button :disabled="isFormInvalid" class="">Actualizar datos</button>
                <!-- <div id="loginerror" :class="loginCorrectly ? 'successful-message' : 'error-message'">{{
                    userError.loginError }}
                </div> -->
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
</style>
