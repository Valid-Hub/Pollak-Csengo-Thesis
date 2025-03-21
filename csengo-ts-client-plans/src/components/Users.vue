<script setup lang="ts">
import { inject, onMounted, ref } from "vue";
import { on } from "@/utils/eventBus.util";
import User from "./User.vue";

const filteredUsers = {
  user: {
    id: 1,
    username: "asd",
    email: "asd",
    role: "asd",
    kreta: {
      om: 123,
      name: "asd",
    },
  },
  user: {
    id: 1,
    username: "asd",
    email: "asd",
    role: "asd",
    kreta: {
      om: 123,
      name: "asd",
    },
  },
  user: {
    id: 1,
    username: "asd",
    email: "asd",
    role: "asd",
    kreta: {
      om: 123,
      name: "asd",
    },
  },
  user: {
    id: 1,
    username: "asd",
    email: "asd",
    role: "asd",
    kreta: {
      om: 123,
      name: "asd",
    },
  },
  user: {
    id: 1,
    username: "asd",
    email: "asd",
    role: "asd",
    kreta: {
      om: 123,
      name: "asd",
    },
  },
};
</script>

<template>
  <div class="full-screen-container">
    <div class="search-container">
      <input type="text" placeholder="Keresés" class="search-bar" />
    </div>
    <div class="table-container">
      <table class="data-table">
        <thead>
          <tr>
            <th>Felhasználónév</th>
            <th>Név</th>
            <th>Email</th>
            <th>OM</th>
            <th>Rang</th>
            <th>Szerkesztés</th>
          </tr>
        </thead>
        <tbody>
          <User v-for="item in filteredUsers" :key="item.id" :user="item" />
        </tbody>
      </table>
    </div>
  </div>
</template>

<style scoped>
.full-screen-container {
  color: black;
  width: 90%;
  height: 90%;
  display: flex;
  flex-direction: column;
  justify-content: start;
  align-items: center;
}

/* And lastly, these are the table styles */
.table-container {
  width: 100%;
  overflow-x: auto;
  scrollbar-width: none;
}

.table-container::-webkit-scrollbar {
  display: none;
}

.data-table th {
  position: sticky;
  top: 0;
  background-color: white;
  z-index: 1;
  font-weight: bold;
}

.data-table thead {
  border-bottom: 4px solid black;
}

.data-table {
  width: 100%;
  border-collapse: collapse;
}

.data-table td {
  padding: 12px;
  text-align: center;
  font-size: 1.3rem;
}

.data-table th {
  border-bottom: 4px solid black;
  font-weight: bold;
}

tr {
  text-align: center;
}

.search-container {
  margin-bottom: 20px;
}

.search-bar {
  font-family: "Anta";
  background-color: white;
  color: black;
  width: 250px;
  padding: 10px;
  font-size: 1.3rem;
  text-align: center;
  border: none;
  border-bottom: 4px solid black;
}

.search-bar:focus {
  outline: none;
}

.upload {
  font-family: "Anta";
  color: white;
  margin-top: 3%;
  font-size: 2rem;
  padding: 10px 30px;
  border-radius: 10px;
  background-color: #3883d9;
}

.upload:hover {
  background-color: #2a64a6;
  cursor: pointer;
}
</style>
