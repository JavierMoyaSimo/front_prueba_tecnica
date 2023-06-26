<script setup>
const goRegister = () => {

}
</script>

<template>
  <div class="home-div">
    <h1>HOME</h1>
    <div class="buttons-div">
      <button @click="goRegister">REGISTER</button>
      <button>LOGIN</button>
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

h1 {
  color: green;
}

.buttons-div {
  display: flex;
  justify-content: space-around;
  align-items: center;
  width: 100%;
}
</style>
