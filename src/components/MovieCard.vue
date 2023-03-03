<script setup>
import { ref } from "vue";
defineProps({
  event: {
    type: Object,
    required: true,
  },
});

const isModalOpen = ref(false);
const movieInfo = ref(null);

async function movieData(imdbID) {
  console.log(imdbID);
    const result = await fetch(
      "https://www.omdbapi.com/?i=" + imdbID + "&apikey=643f294a"
    );
    movieInfo.value = await result.json();
    isModalOpen.value = true;
  }
</script>

<template>
  <div class="event-card" @click="movieData(event.imdbID)">
    <img :src="event.Poster" alt="Poster" />
    <h2>{{ event.Title }}</h2>
    <p>imdbID: {{ event.imdbID }}</p>
    <p>Year: {{ event.Year }}</p>
  </div>
  <template>
    <div class="text-center">
      <v-dialog v-model="isModalOpen" width="auto">
        <v-card>
          <v-card-text>
            <img :src="event.Poster" alt="Poster" />
            <h2>Genre: {{ movieInfo.Genre }}</h2>
            <p>Director: {{ movieInfo.Director }}</p>
            <p>Writer: {{ movieInfo.Writer }}</p>
          </v-card-text>
          <v-card-actions>
            <v-btn color="primary" block @click="isModalOpen = false"
              >Close Dialog</v-btn
            >
          </v-card-actions>
        </v-card>
      </v-dialog>
    </div>
  </template>
</template>

<style scoped>
.event-card {
  padding: 20px;
  width: 300px;
  cursor: pointer;
  border: 1px solid #39495c;
  margin-bottom: 18px;
  text-align: center;
}
.event-card:hover {
  transform: scale(1.01);
  box-shadow: 0 3px 12px 0 rgba(0, 0, 0, 0.2);
}
img {
  width: 250px;
}
</style>
