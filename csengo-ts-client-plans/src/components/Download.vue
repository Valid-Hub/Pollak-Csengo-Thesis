<script setup lang="ts"></script>

<template>
  <div class="full-screen-container">
    <button class="create">Szavazásban lévő zenék letöltése</button>
    <button class="create">Nyertes zene letöltése</button>
  </div>
</template>

<style scoped>
* {
  color: black;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

/* THESE ARE THE TABLE STYLES */
.full-screen-container {
  width: 90%;
  height: 90%;
  display: flex;
  flex-direction: column;
  justify-content: start;
  align-items: center;
}

.create {
  font-family: "Anta", serif;
  color: white;
  margin-top: 3%;
  font-size: 1.6rem;
  padding: 10px 30px;
  border-radius: 10px;
  background-color: #3883d9;
  width: 50dvh;
}

.create:hover {
  background-color: #2a64a6;
  cursor: pointer;
}

.create-start {
  font-family: "Anta", serif;
  color: white;
  margin-top: 3%;
  font-size: 1.6rem;
  padding: 10px 30px;
  border-radius: 10px;
  background-color: #48ff00;
}

.create-start:hover {
  background-color: #50d256;
  cursor: pointer;
}

.create-stop {
  font-family: "Anta", serif;
  color: white;
  margin-top: 3%;
  font-size: 1.6rem;
  padding: 10px 30px;
  border-radius: 10px;
  background-color: #e30102;
}

.create-stop:hover {
  background-color: #b20102;
  cursor: pointer;
}
</style>
