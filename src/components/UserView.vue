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
</script>

<template>
    <div class="userView-div">
        <!-- Rol user -->
        <div v-if="user && user.rol === 'user'">
            <h2>Bienvenido, {{ user && user.email }}!</h2>
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
</style>
