<script setup lang="ts">
import { mdiPencil } from "@mdi/js";
import SvgIcon from "@jamescoyle/vue-icon";
import { inject } from "vue";
import { emit } from "@/utils/eventBus.util";

const props = defineProps<{
  user: {
    id: "";
    username: "";
    email: "";
    role: "";
    kreta: {
      om: "";
      name: "";
    };
  };
}>();
</script>

<template>
  <tr>
    <td>asd</td>
    <td>asd</td>
    <td>asd</td>
    <td>asd</td>
    <td>asd</td>
    <td>
      <button class="edit-button">
        <SvgIcon type="mdi" :path="mdiPencil" class="icon" />
      </button>
    </td>
  </tr>
</template>

<style scoped>
td {
  align-items: center;
  padding: 3px;
}

.play-button,
.edit-button,
.delete-button {
  padding: 5px;
  width: 40px;
  height: 40px;
  border: none;
  border-radius: 10px;
  transition: all 0.2s ease-in-out;
}

.play-button {
  background-color: #2a73c5;
}

.edit-button {
  background-color: #ff00d0;
}

.delete-button {
  background-color: #b61431;
}

.play-button:hover,
.edit-button:hover,
.delete-button:hover {
  transform: scale(1.1);
  cursor: pointer;
}

.play-button:hover {
  background-color: #1c5b8a;
}

.edit-button:hover {
  background-color: #b40193;
}

.delete-button:hover {
  background-color: #9e0b26;
}

.icon {
  color: white;
  width: 30px;
  height: 30px;
  transition: all 0.2s ease-in-out;
}

.play-button:hover .icon,
.edit-button:hover .icon,
.delete-button:hover .icon {
  transform: scale(1.1);
}
</style>
