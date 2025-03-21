<script lang="ts" setup>
import SnipperPanel from "../../components/SnipperPanel.vue";
import { onBeforeUnmount, onMounted, ref } from "vue";
import SvgIcon from "@jamescoyle/vue-icon";
import { mdiHome } from "@mdi/js";

const isWindowSizeEnough = ref(true);

function checkWindowSize() {
  isWindowSizeEnough.value =
    window.innerWidth >= 315 && window.innerHeight >= 315;
}

onMounted(() => {
  checkWindowSize();
  window.addEventListener("resize", checkWindowSize);
});

onBeforeUnmount(() => {
  window.removeEventListener("resize", checkWindowSize);
});

const isDropdownVisible = ref(false);

function toggleDropdown() {
  isDropdownVisible.value = !isDropdownVisible.value;
}

onMounted(() => {});
</script>

<template>
  <div class="main-container background-image">
    <div v-if="isWindowSizeEnough">
      <div class="header">
        <div>
          <RouterLink to="/">
            <SvgIcon type="mdi" :path="mdiHome" class="navbar-icon" />
          </RouterLink>
        </div>
        <div class="user-icon" @click="toggleDropdown">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 24 24"
            fill="white"
            width="48px"
            height="48px"
          >
            <path
              d="M12 2C9.79 2 8 3.79 8 6s1.79 4 4 4 4-1.79 4-4-1.79-4-4-4zm0 10c-4.41 0-8 2.24-8 5v3h16v-3c0-2.76-3.59-5-8-5z"
            />
          </svg>
          <div v-if="isDropdownVisible" class="dropdown-menu">
            <button
              v-if="isAdmin"
              class="dropdown-item"
              @click="navigateToAdmin"
            >
              Admin
            </button>
            <button class="dropdown-item" @click="logout">Kijelentkezés</button>
          </div>
        </div>
      </div>
      <div class="container">
        <div class="card-body move-down">
          <SnipperPanel />
        </div>
      </div>
    </div>
    <div v-else class="centered-message">
      <h3>Ez az oldal nem megtekinthető ekkora kijelzőn</h3>
    </div>
  </div>
</template>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Anta&family=JetBrains+Mono:ital,wght@0,100..800;1,100..800&display=swap");

.background-image {
  background: url("../assets/background.jpg") no-repeat center center fixed;
  background-size: cover;
}

.centered-message {
  font-family: "Anta";
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  text-align: center;
  color: #fff;
}

.main-container {
  height: 100%;
  position: relative;
}

.header {
  position: absolute;
  top: 10px;
  left: 0px;
  right: 30px;
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  font-family: "Anta";
  color: white;
  padding-left: 20px;
  padding-top: 20px;
}

.navbar-icon {
  width: 48px;
  height: 48px;
  color: white;
  cursor: pointer;
}

.user-icon {
  width: 48px;
  height: 48px;
  position: relative;
  cursor: pointer;
}

.dropdown-menu {
  z-index: 10000;
  position: absolute;
  top: 60px;
  right: 0;
  background-color: rgba(0, 0, 0, 0.8);
  color: white;
  padding: 10px;
  border-radius: 5px;
  box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
}

.dropdown-item {
  background: transparent;
  border: none;
  color: white;
  padding: 8px 16px;
  text-align: left;
  cursor: pointer;
  font-size: 1rem;
  transition: background 0.3s ease;
}

.dropdown-item:hover {
  background: rgba(255, 255, 255, 0.1);
}

.card-body p {
  font-size: 1.2rem;
  margin: 0;
}

@media (max-width: 1300px) {
  .user-icon {
    width: 32px;
    height: 32px;
  }
}

.container {
  display: flex;
  justify-content: space-evenly;
  align-items: center;
  min-height: 100vh;
  padding: 20px;
  gap: 50px;
}

.card-body {
  font-family: "Anta";
  position: relative;
  width: 100%;
  max-width: 720px;
  min-width: 600px;
  height: 700px;
  padding: 20px;
  background: rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(50px);
  box-sizing: border-box;
  box-shadow: 0 15px 25px rgba(0, 0, 0, 0.6);
  border-radius: 10px;
  text-align: center;
  color: white;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  word-wrap: break-word;
}

.move-down {
  margin-top: 0px;
}

@media (max-height: 980px) {
  .card-body {
    margin-top: 0px;
  }
}

@media (max-width: 1300px) {
  .card-body {
    height: auto;
    margin-top: 0px;
    min-width: 100%;
    max-width: none;
  }

  .move-down {
    margin-top: 100px;
  }

  .card-body p {
    font-size: 1rem;
  }
}
</style>
