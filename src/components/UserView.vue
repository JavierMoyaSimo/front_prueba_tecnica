<script setup>
import { ref, onMounted, computed } from 'vue'
import { useRouter } from 'vue-router';
import axios from 'axios';

const user = ref(null);
const dataToUpdate = ref(null);
const allUsers = ref(null);

//USER-ERROR-STATE
const userError = ref({
    emailError: '',
    phoneError: '',
    passwordError: '',
    updateError: '',
});

//ENDPOINTS
const endpoint = 'http://localhost:3001/modifyuser/';
const getUsers = 'http://localhost:3001/listusers';
const deleteUserEndpoint = 'http://localhost:3001/deleteUser/';

onMounted(async () => {
    const userData = localStorage.getItem('user');
    if (userData) {
        user.value = JSON.parse(userData);

        dataToUpdate.value = { ...user.value };
    }

    if (user.value.rol === "admin") {

        try {
            allUsers.value = await getAllUsers()
        } catch (error) {
            console.error(error);
        }

        // .then((response) => {
        //     allUsers.value = response;
        // })
        // .catch((error) => console.error(error));
    }
})



//GET ALL USERS
const getAllUsers = async () => {
    try {
        const response = await axios.get(getUsers, {
            headers: { Authorization: `Bearer ${user.value.jwt}` },
        });
        return response.data;

    } catch (error) {
        console.error('Error', error);
    }
};

// DELETE-USER-FUNCTION
const deleteUser = async (email) => {
    try {
        const response = await axios.delete(`${deleteUserEndpoint}${email}`, {
            headers: { Authorization: `Bearer ${user.value.jwt}` },
        });


        deletedUserEmail.value = email;
        if (response.data.message === "User successfully deleted") {

            isDeleted.value[email] = "Usuario eliminado";

            setTimeout(() => {
                router.push('/login');

            }, 500);

            setTimeout(() => {
                router.push('/userView');

            }, 501);
        }




    } catch (error) {
        console.error('Error deleting user', error);
    }
}

const isDeleted = ref({});
const deletedUserEmail = ref(null);

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

        const response = await axios.patch(endpoint + user.value.email, {
            name: dataToUpdate.value.name,
            phone: dataToUpdate.value.phone,
            password: dataToUpdate.value.password,
        }, {
            headers: { Authorization: `Bearer ${user.value.jwt}` },
        });

        if (response.data.message === "User not found") {
            userError.value.updateError = 'Error en la actualización, revisa los campos';
            document.getElementById('updateError').textContent = userError.value.updateError;
            return;
        }

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
    <div v-if="user" class="userView-div">

        <div class="border">
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
        </div>

        <div class="border">
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



    </div>

    <!-- Rol admin -->
    <div v-if="user && user.rol === 'admin'" class="userView-div">
        <div class="border">
            <h2>LISTA DE USUARIOS </h2>


            <div v-if="allUsers">
                <div v-for="user in allUsers" :key="user._id" class="user-item" v-if="user.email !== deletedUserEmail">
                    <div class="row-div">
                        <p class="label">Nombre:</p>
                        <p>{{ user.name }}</p>
                    </div>
                    <div class="row-div">
                        <p class="label">Email:</p>
                        <p>{{ user.email }}</p>
                    </div>
                    <div class="row-div">
                        <p class="label">Teléfono:</p>
                        <p>{{ user.phone }}</p>
                    </div>
                    <button class="delete-button" @click="deleteUser(user.email)">Eliminar usuario</button>
                    <div :id="user._id" class='error-message'>
                        {{ isDeleted[user.email] }}</div>

                </div>
            </div>
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

.border {
    border-radius: 10px;
    border: 1px solid gray;
    padding: 1em;
    background-color: rgba(37, 132, 167, 0.575);
    margin-top: 1em;
    margin-bottom: 1em;
}

.row-div {
    display: flex;
    justify-content: space-between;
}

.label {
    font-weight: bold;
    font-family: Georgia, 'Times New Roman', Times, serif;
    margin-right: 1em;
}


button {
    margin-top: 1em;
    margin-bottom: 1em;
}

.error-message {
    color: red;
    font-size: small;
}

.successful-message {
    color: green;
    font-size: small;
}

.user-item {
    border: 1px solid black;
    background-color: rgba(218, 201, 201, 0.47);
    border-radius: 10px;
    padding: 1em;
    margin-bottom: 1em;
    text-align: center;
}

.delete-button {
    cursor: pointer;
    color: red;
}
</style>
