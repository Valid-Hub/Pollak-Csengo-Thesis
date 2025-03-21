<template>
  <div class="chart-container">
    <div class="chart">
      <canvas class="vote-chart" id="voteChart"></canvas>
    </div>
    <div class="legend">
      <div class="timer">
        <h1>Frissítés: {{ formattedCountdown }}</h1>
        <h1 class="connection-title">Színek -- zenék</h1>
      </div>
      <ul class="title-container">
        <li v-for="(song, index) in songs" :key="song.songId">
          <span
            :style="{ backgroundColor: colors[index] }"
            class="color-box"
          ></span>
          <span class="song-title"
            >{{ song.songTitle }} - {{ song.voteCount }}</span
          >
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import { ref, computed, onMounted, onUpdated } from "vue";
import Chart from "chart.js/auto";
import ChartDataLabels from "chartjs-plugin-datalabels";
Chart.register(ChartDataLabels);

export default {
  setup() {
    const songs = ref([
      {
        songId: "e33d8a9e-506b-4423-be15-4bccbca941fe",
        songTitle: "Alma20",
        voteCount: 121,
      },
      {
        songId: "9a380d38-3537-4b4d-8e8e-4dafb9fad375",
        songTitle: "zene1",
        voteCount: 110,
      },
      {
        songId: "a5962c85-09de-4115-a7d5-7a8811ac25f6",
        songTitle: "zene2",
        voteCount: 140,
      },
      {
        songId: "b2fbe141-f3f1-46a7-b3af-16d6a0c0baf9",
        songTitle: "alma",
        voteCount: 160,
      },
      {
        songId: "d09a3d33-780f-40a1-a591-83e87e31dc5f",
        songTitle: "title",
        voteCount: 120,
      },
      {
        songId: "969bf7e2-d5f0-49a5-856c-cab9d92900dc",
        songTitle: "asdawgauaa",
        voteCount: 11,
      },
      {
        songId: "e33d8a9e-506b-4423-be15-4bccbca941fe",
        songTitle: "Alma20",
        voteCount: 121,
      },
      {
        songId: "9a380d38-3537-4b4d-8e8e-4dafb9fad375",
        songTitle: "zene1",
        voteCount: 110,
      },
      {
        songId: "a5962c85-09de-4115-a7d5-7a8811ac25f6",
        songTitle: "zene2",
        voteCount: 140,
      },
      {
        songId: "b2fbe141-f3f1-46a7-b3af-16d6a0c0baf9",
        songTitle: "alma",
        voteCount: 160,
      },
      {
        songId: "d09a3d33-780f-40a1-a591-83e87e31dc5f",
        songTitle: "title",
        voteCount: 120,
      },
      {
        songId: "969bf7e2-d5f0-49a5-856c-cab9d92900dc",
        songTitle: "abkfluaa",
        voteCount: 11,
      },
      {
        songId: "e33d8a9e-506b-4423-be15-4bccbca941fe",
        songTitle: "Alma20",
        voteCount: 121,
      },
      {
        songId: "9a380d38-3537-4b4d-8e8e-4dafb9fad375",
        songTitle: "zene1",
        voteCount: 110,
      },
      {
        songId: "a5962c85-09de-4115-a7d5-7a8811ac25f6",
        songTitle: "zene2",
        voteCount: 140,
      },
      {
        songId: "b2fbe141-f3f1-46a7-b3af-16d6a0c0baf9",
        songTitle: "alma",
        voteCount: 160,
      },
      {
        songId: "e33d8a9e-506b-4423-be15-4bccbca941fe",
        songTitle: "Alma20",
        voteCount: 121,
      },
      {
        songId: "9a380d38-3537-4b4d-8e8e-4dafb9fad375",
        songTitle: "zene1",
        voteCount: 110,
      },
      {
        songId: "a5962c85-09de-4115-a7d5-7a8811ac25f6",
        songTitle: "zene2",
        voteCount: 140,
      },
      {
        songId: "b2fbe141-f3f1-46a7-b3af-16d6a0c0baf9",
        songTitle: "alma",
        voteCount: 160,
      },
      {
        songId: "d09a3d33-780f-40a1-a591-83e87e31dc5f",
        songTitle: "title",
        voteCount: 120,
      },
      {
        songId: "e33d8a9e-506b-4423-be15-4bccbca941fe",
        songTitle: "Alma20",
        voteCount: 121,
      },
      {
        songId: "9a380d38-3537-4b4d-8e8e-4dafb9fad375",
        songTitle: "zene1",
        voteCount: 110,
      },
      {
        songId: "a5962c85-09de-4115-a7d5-7a8811ac25f6",
        songTitle: "zene2",
        voteCount: 140,
      },
      {
        songId: "b2fbe141-f3f1-46a7-b3af-16d6a0c0baf9",
        songTitle: "alma",
        voteCount: 160,
      },
      {
        songId: "d09a3d33-780f-40a1-a591-83e87e31dc5f",
        songTitle: "title",
        voteCount: 120,
      },
      {
        songId: "d09a3d33-780f-40a1-a591-83e87e31dc5f",
        songTitle: "title",
        voteCount: 120,
      },
      {
        songId: "969bf7e2-d5f0-49a5-856c-cab9d92900dc",
        songTitle: "asdawgagaa",
        voteCount: 11,
      },
    ]);

    const getRandomColor = () => {
      const letters = "0123456789ABCDEF";
      let color = "#";
      for (let i = 0; i < 6; i++) {
        color += letters[Math.floor(Math.random() * 16)];
      }
      return color;
    };

    const colors = ref(songs.value.map(() => getRandomColor()));

    let chartInstance = null;

    const createChart = () => {
      if (chartInstance) {
        chartInstance.destroy();
      }

      const ctx = document.getElementById("voteChart").getContext("2d");
      chartInstance = new Chart(ctx, {
        type: "bar",
        data: {
          labels: songs.value.map((song) => song.songTitle),
          datasets: [
            {
              label: "Szavazatok",
              data: songs.value.map((song) => song.voteCount),
              backgroundColor: colors.value,
              borderRadius: { topLeft: 10, topRight: 10 },
              borderSkipped: false,
            },
          ],
        },
        options: {
          responsive: true,
          plugins: {
            legend: {
              display: false,
            },
            datalabels: {
              color: "white",
              font: {
                size: 20,
              },
              anchor: "end",
              align: "top",
              formatter: (value) => value,
            },
          },
          scales: {
            x: {
              ticks: {
                color: "white",
                font: {
                  size: 20,
                  weight: "bold",
                },
              },
              title: {
                display: true,
                color: "white",
                text: "Zenék",
                font: {
                  size: 20,
                  weight: "bold",
                },
              },
            },
            y: {
              title: {
                display: true,
                text: "Szavazatok száma",
                color: "white",
                font: {
                  size: 20,
                  weight: "bold",
                },
              },
              beginAtZero: true,
              suggestedMax:
                Math.max(...songs.value.map((song) => song.voteCount)) + 1,
            },
          },
        },
        plugins: [ChartDataLabels],
      });
    };

    const countdown = ref(300);

    const formattedCountdown = computed(() => {
      const minutes = Math.floor(countdown.value / 60);
      const seconds = countdown.value % 60;
      return `${minutes}:${seconds.toString().padStart(2, "0")}`;
    });

    const startCountdown = () => {
      setInterval(() => {
        if (countdown.value > 0) {
          countdown.value--;
        } else {
          countdown.value = 300;
          fetchSongs();
        }
      }, 1000);
    };

    const fetchSongs = () => {
      console.log("Frissítés történt!");
      createChart();
    };

    const startAutoScroll = () => {
      const legend = document.querySelector(".legend");
      let scrollDirection = 1;

      let scrollInterval = setInterval(() => {
        if (legend) {
          if (legend.scrollTop + legend.clientHeight >= legend.scrollHeight) {
            scrollDirection = -1;
          } else if (legend.scrollTop <= 0) {
            scrollDirection = 1;
          }

          legend.scrollTop += scrollDirection;
        }
      }, 30);
    };

    onMounted(() => {
      songs.value.sort((a, b) => b.voteCount - a.voteCount);
      createChart();
      startCountdown();
      startAutoScroll();
    });

    return {
      songs,
      colors,
      countdown,
      formattedCountdown,
    };
  },
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Anta&family=JetBrains+Mono:ital,wght@0,100..800;1,100..800&display=swap");

* {
  font-family: "Anta";
}

.timer {
  text-align: center;
  font-size: 2rem;
}

.timer,
h1 {
  position: sticky;
  top: 0;
  background-color: black;
  color: white;
  z-index: 2;
  margin: 0;
}

.connection-title {
  text-align: center;
  font-size: 2rem;
}

.legend {
  flex: 1;
  overflow-y: auto;
  max-height: calc(100vh - 100px);
  scrollbar-width: none;
  scroll-behavior: smooth;
}

.chart-container {
  color: white;
  background-color: black;
  display: flex;
  gap: 20px;
  padding: 20px;
  height: 100vh;
  box-sizing: border-box;
}

.chart {
  flex: 3;
  display: flex;
  align-items: center;
  justify-content: center;
}

.color-box {
  display: inline-block;
  width: 20px;
  height: 20px;
  margin-right: 10px;
  border-radius: 3px;
}

.song-title {
  font-size: 1.5rem;
  font-weight: bold;
}

body,
html {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
}
</style>
