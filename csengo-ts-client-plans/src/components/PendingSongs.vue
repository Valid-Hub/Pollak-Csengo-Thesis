<template>
    <div class="full-screen-container">
        <!-- THIS IS THE SEARCH BAR -->
        <div class="search-container">
            <input v-model="searchQuery" type="text" placeholder="Keresés" class="search-bar" />
        </div>


        <!-- THIS IS THE TABLE -->
        <div class="table-container">
            <table class="data-table">
                <thead>
                    <tr>
                        <th>Név</th>
                        <th>Feltöltés ideje</th>
                        <th>Lejátszás</th>
                        <th>Engedélyezés</th>
                        <th>Elutasitás</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(item, index) in filteredItems" :key="index">
                        <td>{{ item.title }}</td>
                        <td>{{ item.createdAt }}</td>
                        <td>
                            <button class="play-button" @click="this.playVideo(item)">
                                <SvgIcon type="mdi" :path="isPlaying[item.id] ? mdiPause : mdiPlay" class="icon" />
                            </button>
                        </td>
                        <td>
                            <button class="edit-button">
                                <SvgIcon type="mdi" :path="mdiCheck" class="icon" />
                            </button>
                        </td>
                        <td>
                            <button class="delete-button">
                                <SvgIcon type="mdi" :path="mdiClose" class="icon" />
                            </button>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</template>

<script>
import SvgIcon from "@jamescoyle/vue-icon";
import { mdiCheck, mdiPlay, mdiPause, mdiClose } from "@mdi/js";

export default {
    components: {
        SvgIcon,
    },
    data() {
        return {
            isPlaying: {},
            mdiCheck,
            mdiPlay,
            mdiPause,
            mdiClose,
            searchQuery: '',
            items: []
        };
    },
    computed: {
        filteredItems() {
            return this.searchQuery
                ? this.items.filter(item =>
                    item.title.toLowerCase().includes(this.searchQuery.toLowerCase())
                )
                : this.items;
        }

    },
    async mounted() {
        this.fetchPendingSongs()
    },
    methods: {
        playVideo(item) {
            this.isPlaying[item.id] = !this.isPlaying[item.id];
        },
        fetchPendingSongs() {
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
            ]
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




/* These are the styles for the search bar */
.search-container {
    margin-bottom: 20px;
}

.search-bar {
    font-family: 'Anta';
    color: black;
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






/* These are the style for the table and its button */
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
    background-color: #00C700;
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
    background-color: #01a901;
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