<template>
    <div class="full-screen-container">
        <!-- THE SEARCH BAR TO SEARCH MUSIC BY NAME -->
        <div class="search-container">
            <input v-model="searchQuery" type="text" placeholder="Keresés" class="search-bar" />
        </div>

        <!-- THE TABLE TO LIST TEH SONGS -->
        <div class="table-container">
            <table class="data-table">
                <thead>
                    <tr>
                        <th>Név</th>
                        <th>Feltöltés ideje</th>
                        <th>Lejátszás</th>
                        <th>Szerkesztés</th>
                        <th>Törlés</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(item, index) in filteredItems" :key="index">
                        <td>{{ item.title }}</td>
                        <td>{{ item.createdAt }}</td>
                        <td>
                            <button class="play-button" @click="playVideo(item)">
                                <SvgIcon type="mdi" :path="isPlaying[item.id] ? mdiPause : mdiPlay" class="icon" />
                            </button>
                        </td>
                        <td>
                            <button class="edit-button" @click="openEdit(item)">
                                <SvgIcon type="mdi" :path="mdiPencil" class="icon" />
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

        <button class="upload" @click="openCreate()">Feltöltés</button>



        <!-- THE POPUP TO UPLOAD MUSIC -->
        <div v-if="isCreating" class="overlay">
            <div class="create-card">
                <h2>Hang létrehozása</h2>
                <form @submit.prevent="create">
                    <input id="name" v-model="name" type="text" placeholder="Adja meg a nevet" />

                    <div v-if="soundName" class="file-name">{{ soundName }}</div>
                    <input id="sound" type="file" accept=".mp3" @change="onFileChange" class="hidden-input" />
                    <label for="sound" class="file-upload-button">Válasszon fájlt</label>

                    <button type="submit" @click="create()">Létrehozás</button>
                    <button type="button" @click="closeCreate()">Mégse</button>
                </form>
            </div>
        </div>

        <!-- THE POPUP TO RENAME MUSIC -->
        <div v-if="isEditing" class="overlay">
            <div class="edit-card">
                <h2>Átnevezés</h2>
                <form @submit.prevent="">
                    <input id="name" v-model="name" type="text" placeholder="Adja meg a nevet" />

                    <button type="submit" @click="rename()">Átnevezés</button>
                    <button type="button" @click="closeEdit()">Mégse</button>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
import SvgIcon from "@jamescoyle/vue-icon";
import { mdiPencil, mdiDelete, mdiPlay, mdiPause, mdiMagnify } from "@mdi/js";

export default {
    components: {
        SvgIcon,
    },
    data() {
        return {
            // These are responsible for the popup visibility
            isEditing: false,
            isCreating: false,

            // These are responsible for the music data
            name: '',
            soundName: '',
            sound: null,

            // These are responsible for the table
            isPlaying: {},
            mdiPencil,
            mdiDelete,
            mdiPlay,
            mdiPause,
            mdiMagnify,
            searchQuery: '',
            items: []
        };
    },
    computed: {
        filteredItems() {
            return this.searchQuery ? this.items.filter(item => item.title.toLowerCase().includes(this.searchQuery.toLowerCase())) : this.items;
        }
    },
    async mounted() {
        this.fetchSongs();
    },
    methods: {
        playVideo(item) {
            this.isPlaying[item.id] = !this.isPlaying[item.id];
        },
        onFileChange(event) {
            const file = event.target.files[0];
            this.sound = file;
            this.soundName = file ? file.name : '';
        },
        openCreate() {
            this.isCreating = true;
        },
        create() {
            console.log(this.sound)
            console.log(this.soundName)
            this.isCreating = false
            this.sound = null
            this.soundName = null
        },
        rename() {
            console.log(this.name)
            this.name = null
            this.isEditing = false
        },
        closeCreate() {
            this.isCreating = false
            this.sound = null
            this.soundName = null
        },
        openEdit(item) {
            this.isEditing = true
            this.name = item.title
        },
        closeEdit() {
            this.isEditing = false
        },
        fetchSongs() {
            this.items = [
                { id: 'awdwad', title: 'Shape of You', createdAt: '2024.06.25', updatedAt: '2024.06.25' },
                { id: 'segseg', title: 'Requiem by who know', createdAt: '2024.06.25', updatedAt: '2024.06.25' },
                { id: 'Ashdrhdrh1', title: 'Montagem Coral', createdAt: '2024.06.25', updatedAt: '2024.06.25' },
                { id: 'Adhrh2024.06.25', title: 'Montagem whatever', createdAt: '2024.06.25', updatedAt: '2024.06.25' },
                { id: '346', title: 'Shape of You', createdAt: '2024.06.25', updatedAt: '2024.06.25' },
                { id: '2342', title: 'Requiem by who know', createdAt: '2024.06.25', updatedAt: '2024.06.25' },
                { id: '3746', title: 'Montagem Coral', createdAt: '2024.06.25', updatedAt: '2024.06.25' },
                { id: '76474d', title: 'Montagem whatever', createdAt: '2024.06.25', updatedAt: '2024.06.25' },
                { id: 'awdwada', title: 'Shape of You', createdAt: '2024.06.25', updatedAt: '2024.06.25' },
                { id: 'segsega', title: 'Requiem by who know', createdAt: '2024.06.25', updatedAt: '2024.06.25' },
                { id: 'Ashdrhdrh1a', title: 'Montagem Coral', createdAt: '2024.06.25', updatedAt: '2024.06.25' },
                { id: 'Adhrh2024.06.25a', title: 'Montagem whatever', createdAt: '2024.06.25', updatedAt: '2024.06.25' },
                { id: '34a', title: 'Shape of You', createdAt: '2024.06.25', updatedAt: '2024.06.25' },
                { id: '2342a', title: 'Requiem by who know', createdAt: '2024.06.25', updatedAt: '2024.06.25' },
                { id: '3746a', title: 'Montagem Coral', createdAt: '2024.06.25', updatedAt: '2024.06.25' },
                { id: '76474da', title: 'Montagem whatever', createdAt: '2024.06.25', updatedAt: '2024.06.25' },
                { id: 'awdwadj', title: 'Shape of You', createdAt: '2024.06.25', updatedAt: '2024.06.25' },
                { id: 'segsegj', title: 'Requiem by who know', createdAt: '2024.06.25', updatedAt: '2024.06.25' },
                { id: 'Ashdrhdrh1j', title: 'Montagem Coral', createdAt: '2024.06.25', updatedAt: '2024.06.25' },
                { id: 'Adhrh2024.06.25j', title: 'Montagem whatever', createdAt: '2024.06.25', updatedAt: '2024.06.25' },
                { id: '346j', title: 'Shape of You', createdAt: '2024.06.25', updatedAt: '2024.06.25' },
                { id: '2342j', title: 'Requiem by who know', createdAt: '2024.06.25', updatedAt: '2024.06.25' },
                { id: '3746j', title: 'Montagem Coral', createdAt: '2024.06.25', updatedAt: '2024.06.25' },
                { id: '76474dj', title: 'Montagem whatever', createdAt: '2024.06.25', updatedAt: '2024.06.25' },
            ];
        }
    }
};
</script>


<style scoped>
/* This is the container of the whole component which is changed by the navbar, absolutely needed */
.full-screen-container {
    color: black;
    width: 90%;
    height: 90%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
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

.hidden-input {
    display: none;
}

.file-upload-button {
    font-family: 'Anta';
    background-color: #3883d9;
    color: white;
    width: 250px;
    padding: 10px;
    font-size: 1.3rem;
    text-align: center;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease-in-out;
}

.file-upload-button:hover {
    background-color: #2a64a6;
}






/* These are the styles for the search bar */
.search-container {
    margin-bottom: 20px;
}

.search-bar {
    font-family: 'Anta';
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

/* These are the style for the icon buttons in the table */
.play-button,
.edit-button,
.delete-button {
    width: 40px;
    height: 40px;
    border: none;
    border-radius: 10px;
    transition: all 0.2s ease-in-out;
}

.play-button {
    background-color: #2A73C5;
}

.edit-button {
    background-color: #FF00D0;
}

.delete-button {
    background-color: #B61431;
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







/* This is the button under the table */
.upload {
    font-family: 'Anta';
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