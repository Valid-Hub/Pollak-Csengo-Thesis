<template>
    <div class="full-screen-container">
        <div class="search-container">
            <input v-model="searchQuery" type="text" placeholder="Keresés" class="search-bar" />
        </div>

        <div class="table-container">
            <table class="data-table">
                <thead>
                    <tr>
                        <th>Kezdet</th>
                        <th>Vég</th>
                        <th>Zenék</th>
                        <th>Szerkesztés</th>
                        <th>Törlés</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(item, index) in filteredItems" :key="index">
                        <td>{{ item.start }}</td>
                        <td>{{ item.end }}</td>
                        <td>
                            <button class="view-button" @click="this.openView(item)">
                                Megtekintés
                            </button>
                        </td>
                        <td>
                            <button class="edit-button">
                                <SvgIcon type="mdi" :path="mdiPencil" @click="this.openEdit(item)" class="icon" />
                            </button>
                        </td>
                        <td>
                            <button class="delete-button">
                                <SvgIcon type="mdi" :path="mdiDelete" class="icon" />
                            </button>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>

        <button class="create" @click="openCreate()">Létrehozás</button>

        <!-- THE POPUP TO CREATE SESSION -->
        <div v-if="isCreating" class="overlay">
            <div class="create-card">
                <h2>Szavazás létrehozása</h2>
                <form @submit.prevent="create">

                    <label for="startDate">Kezdő dátum:</label>
                    <input id="startDate" v-model="startDate" type="date" required />

                    <label for="endDate">Befejező dátum:</label>
                    <input id="endDate" v-model="endDate" type="date" required />

                    <label for="songDropdown">Válasszon zenéket:</label>
                    <div class="dropdown">
                        <button type="button" @click="toggleDropdown" class="dropdown-button">
                            Zenék kiválasztása
                        </button>
                        <ul v-if="dropdownOpen" class="dropdown-menu">
                            <li v-for="song in songs" :key="song.id">
                                <input type="checkbox" :id="`song-${song.id}`" v-model="selectedSongsMap[song.id]" />
                                <label :for="`song-${song.id}`">{{ song.title }}</label>
                            </li>
                        </ul>
                    </div>

                    <button type="submit">Létrehozás</button>
                    <button type="button" @click="closeCreate">Mégse</button>
                </form>
            </div>
        </div>

        <!-- THE POPUP TO EDIT SESSION -->
        <div v-if="isEditing" class="overlay">
            <div class="edit-card">
                <h2>Szavazás szerkesztése</h2>
                <form @submit.prevent="editSession">
                    <label for="startDateEdit">Kezdő dátum:</label>
                    <input id="startDateEdit" v-model="startDate" type="date" required />

                    <label for="endDateEdit">Befejező dátum:</label>
                    <input id="endDateEdit" v-model="endDate" type="date" required />

                    <label for="songDropdownEdit">Válasszon zenéket:</label>
                    <div class="dropdown">
                        <button type="button" @click="toggleDropdown" class="dropdown-button">
                            Zenék kiválasztása
                        </button>
                        <ul v-if="dropdownOpen" class="dropdown-menu">
                            <li v-for="song in songs" :key="song.id">
                                <input type="checkbox" :id="`song-${song.id}`" v-model="selectedSongsMap[song.id]" />
                                <label :for="`song-${song.id}`">{{ song.title }}</label>
                            </li>
                        </ul>
                    </div>

                    <button type="submit">Módosítás</button>
                    <button type="button" @click="closeEdit">Mégse</button>
                </form>
            </div>
        </div>

        <!-- THE POPUP TO VIEW SONGS -->
        <div v-if="isViewing" class="overlay">
            <div class="view-card">
                <h2>Zenék</h2>
                <div class="table-container">
                    <table class="data-table">
                        <thead>
                            <tr>
                                <th>Név</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="(item, index) in filteredSongs" :key="index">
                                <td>{{ item }}</td>
                            </tr>
                        </tbody>
                    </table>
                </div>
                <button type="button" class="close-button" @click="closeView()">Bezár</button>
            </div>
        </div>
    </div>
</template>

<script>
import SvgIcon from "@jamescoyle/vue-icon";
import { mdiPencil, mdiDelete, mdiPlay, mdiPause } from "@mdi/js";

export default {
    components: {
        SvgIcon,
    },
    data() {
        return {
            isCreating: false,
            isEditing: false,
            isViewing: false,
            currentlyViewingId: null,
            mdiPencil,
            mdiDelete,
            mdiPlay,
            mdiPause,
            searchQuery: '',
            isPlaying: {},
            items: [],
            filteredSongs: [],
            songs: [],
            startDate: '',
            endDate: '',
            dropdownOpen: false,
            selectedSongs: [],
            selectedSongsMap: {},
            editingSession: null,  // Add a property to hold the session being edited
        };
    },
    computed: {
        filteredItems() {
            if (this.searchQuery === '') {
                return this.items;
            } else {
                return this.items.filter(item => item.start.toLowerCase().includes(this.searchQuery.toLowerCase()));
            }
        },
    },
    async mounted() {
        this.fetchSessions()
        this.fetchSongs()
    },
    methods: {
        toggleDropdown() {
            this.dropdownOpen = !this.dropdownOpen;
        },
        create() {
            if (!this.startDate || !this.endDate || Object.keys(this.selectedSongsMap).length === 0) {
                alert("Kérjük, töltsön ki minden mezőt!");
                return;
            }

            console.log("Kezdő dátum:", this.startDate);
            console.log("Befejező dátum:", this.endDate);
            console.log("Kiválasztott zenék:", this.selectedSongsMap);

            this.isCreating = false;
            this.startDate = '';
            this.endDate = '';
            this.selectedSongsMap = {};
        },
        closeCreate() {
            this.isCreating = false;
            this.startDate = '';
            this.endDate = '';
            this.selectedSongsMap = {};
            this.selectedSongs = [];
            this.dropdownOpen = false
        },
        editSession() {
            // Save the edited session
            const editedSession = {
                ...this.editingSession,  // Keep the existing session's properties
                start: this.startDate,   // Update with the new data
                end: this.endDate,
                songNames: Object.keys(this.selectedSongsMap).filter(songId => this.selectedSongsMap[songId]),
            };

            console.log("Edited session:", editedSession);

            this.isEditing = false;
            this.editingSession = null;
            this.startDate = '';
            this.endDate = '';
            this.selectedSongsMap = {};
        },
        closeEdit() {
            this.isEditing = false;
            this.editingSession = null;
        },
        closeView() {
            this.isViewing = false
            this.currentlyViewingId = null
        },
        openEdit(item) {
            this.isEditing = true;
            this.editingSession = { ...item };
            this.startDate = item.start;
            this.endDate = item.end;
            this.selectedSongsMap = {};

        },
        openCreate() {
            this.isCreating = true;
        },
        openView(item) {
            let session = this.items.filter(session => session.id == item.id)
            this.filteredSongs = session[0].songNames;
            this.isViewing = true
        },
        fetchSongs() {
            this.songs = [
                { id: 'awdwad', title: 'Shape of You' },
                { id: 'segseg', title: 'Requiem by who know' },
                { id: 'Ashdrhdr', title: 'Marhachee' },
            ];
        },
        fetchSessions() {
            this.items = [
                {
                    id: 1,
                    start: "2024-10-30",
                    end: "2024-11-05",
                    songNames: ["Shape of You", "Requiem by who know"]
                },
                {
                    id: 2,
                    start: "2024-10-15",
                    end: "2024-11-12",
                    songNames: ["Marhachee"]
                }
            ]
        }
    },
};
</script>


<style scoped>
* {
    color: black;
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}




/* This is the overlay for the popups */
.overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 1000;
}








/* These are the style for the edit popup */
.edit-card {
    background-color: white;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    text-align: center;
    width: 300px;
}

.edit-card h2 {
    margin-bottom: 20px;
    font-size: 1.5rem;
}

.edit-card form {
    display: flex;
    flex-direction: column;
    gap: 10px;
}

.edit-card input,
.edit-card button {
    font-family: 'Anta';
    background-color: white;
    width: 250px;
    padding: 10px;
    font-size: 1.3rem;
    text-align: center;
    border: none;
}

.edit-card button {
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease-in-out;
}

.edit-card input {
    border-bottom: 4px solid black;
}

.edit-card input:focus {
    outline: none;
}

.edit-card button[type="submit"] {
    background-color: #4caf50;
    color: white;
}

.edit-card button[type="button"] {
    background-color: #f44336;
    color: white;
}

.edit-card button[type="submit"]:hover {
    background-color: #358b38;
}

.edit-card button[type="button"]:hover {
    background-color: #be2e24;
}




/* These are the style for the view popup */
.view-card {
    background-color: white;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    text-align: center;
    width: 400px;
    max-height: 400px;
    overflow-y: auto;
    display: flex;
    flex-direction: column;
    gap: 10px;
}


.view-card h2 {
    margin-bottom: 20px;
    font-size: 1.5rem;
}

.view-card button {
    font-family: 'Anta';
    background-color: white;
    width: 100%;
    padding: 10px;
    font-size: 1.3rem;
    text-align: center;
    border: none;
}

.view-card button {
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease-in-out;
}


.view-card button[type="button"] {
    background-color: #f44336;
    color: white;
}

.view-card button[type="button"]:hover {
    background-color: #be2e24;
}







/* These are the styles for the create popup */
.create-card {
    background-color: white;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    text-align: center;
    width: 300px;
}

.create-card h2 {
    margin-bottom: 20px;
    font-size: 1.5rem;
}

.create-card form {
    display: flex;
    flex-direction: column;
    gap: 10px;
}

.create-card input,
.create-card button {
    font-family: 'Anta';
    background-color: white;
    width: 250px;
    padding: 10px;
    font-size: 1.3rem;
    text-align: center;
    border: none;
}

.create-card button {
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease-in-out;
}

.create-card input {
    border-bottom: 4px solid black;
}

.create-card input:focus {
    outline: none;
}

.create-card button[type="submit"] {
    background-color: #4caf50;
    color: white;
}

.create-card button[type="button"] {
    background-color: #f44336;
    color: white;
}

.create-card button[type="submit"]:hover {
    background-color: #358b38;
}

.create-card button[type="button"]:hover {
    background-color: #be2e24;
}

.dropdown {
    position: relative;
}

.dropdown-button {
    padding: 10px;
    background-color: #4caf50;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

.dropdown-menu {
    position: absolute;
    top: 100%;
    left: 0;
    background-color: white;
    border: 1px solid #ccc;
    border-radius: 5px;
    list-style: none;
    padding: 10px;
    z-index: 1000;
}

.dropdown-menu li {
    width: 230px;
    display: flex;
    margin-bottom: 5px;
}

.dropdown-menu input {
    width: 30px;
    margin-right: 20px;
}






/* THESE ARE THE TABLE STYLES */
.edit-button,
.delete-button {
    width: 40px;
    height: 40px;
    border: none;
    border-radius: 10px;
    transition: all 0.2s ease-in-out;
}

.view-button {
    font-family: 'Anta';
    color: white;
    font-size: 1.3rem;
    padding: 5px 15px;
    border-radius: 10px;
    background-color: #3883d9;
    transition: all 0.2s ease-in-out;
}

.edit-button {
    background-color: #FF00D0;
}

.delete-button {
    background-color: #B61431;
}

.view-button:hover,
.edit-button:hover,
.delete-button:hover {
    transform: scale(1.1);
    cursor: pointer;
}

.view-button:hover {
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

.view-button:hover .icon,
.edit-button:hover .icon,
.delete-button:hover .icon {
    transform: scale(1.1);
}

.full-screen-container {
    width: 90%;
    height: 90%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}

.search-container {
    margin-bottom: 20px;
}

.search-bar {
    font-family: 'Anta';
    background-color: white;
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

.create {
    font-family: 'Anta';
    color: white;
    margin-top: 3%;
    font-size: 2rem;
    padding: 10px 30px;
    border-radius: 10px;
    background-color: #3883d9;
}

.create:hover {
    background-color: #2a64a6;
    cursor: pointer;
}

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

.data-table th,
.data-table td {
    padding: 12px;
    text-align: center;
    font-size: 1.3rem;
}

.data-table th {
    border-bottom: 4px solid black;
    font-weight: bold;
}
</style>