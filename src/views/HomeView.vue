<script setup>
import EventCard from "@/components/MovieCard.vue";
import { ref } from "vue";
import axios from "axios";

const events = ref(null);
const query = ref("");
const pages = ref(1);
const showBtn = ref(false);

function apiCall(query, pages) {
  axios
    .get(
      "https://www.omdbapi.com/?s=" +
        query +
        "&page=" +
        pages +
        "&apikey=643f294a"
    )
    .then((response) => {
      console.log(response.data);
      if (response.data.Response == "True") {
        events.value = response.data.Search;
        showBtn.value = true;
      } else showBtn.value = false;
    })
    .catch((error) => {
      console.log(error);
    });
}

function changeHandler(e) {
  if (e.target.value.length > 0) {
    query.value = e.target.value;
  }
  apiCall(query.value);
  //e.target.value = '';
}

function changePage() {
  pages.value += 1;
  apiCall(query.value, pages.value);
}
</script>

<template>
  <div>
    <form @submit.prevent="" :onKeyup="changeHandler">
      <input class="input" type="text" placeholder="search movie" />
    </form>
  </div>
  <div class="home">
    <EventCard v-for="event in events" :key="event.imdbID" :event="event" />
  </div>
  <div class="btn">
    <v-btn class="btn" v-if="showBtn" @click="changePage()"> Button </v-btn>
  </div>
</template>

<style>
.home {
  display: flex;
  flex-direction: row;
  /*align-items: center;*/
  flex-wrap: wrap;
  justify-content: center;
  gap: 10px;
  min-width: 400px;
}
form {
  text-align: center;
  margin-bottom: 10px;
}
.input {
  border: 1px solid black;
}
.btn {
  display: flex;
  justify-content: center;
}
</style>
