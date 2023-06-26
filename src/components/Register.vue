<script setup>
//Aqui van los imports

</script>

<template>
    <div class="form-div">
        <h2>BIENVENID@ </h2>
        <!-- NAME -->
        <div class="form-div">
            <input type="text" class="" placeholder="Nombre" required>
            <div class="error-message">{{ }}</div>
        </div>
        <!-- PHONE -->
        <div class="form-div">
            <input type="text" class="" placeholder="Teléfono" required>
            <div class="error-message">{{ }}</div>
        </div>
        <!-- EMAIL -->
        <div class="form-div">
            <input type="email" class="form-control" placeholder="Email" required>
            <div class="error-message">{{ }}</div>
        </div>
        <!-- PASSWORD -->
        <div class="form-div">
            <input :type="passwordShown ? 'text' : 'password'" class="form-control" placeholder="Contraseña" required>
            <div class="error-message">{{ }}</div>
        </div>
        <!-- BUTTON-REGISTER -->
        <button class="btn btn-primary">Registrarme</button>
        <div id="regerror" class="error-message">{{ }}</div>
    </div>
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
</style>





