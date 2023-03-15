<script setup>
import axios from "axios";
import { ref } from "vue";

const moviesDB = ref(null);
const inputValue = ref("");

const API_URL = ref(
  `https://api.themoviedb.org/3/discover/movie?sort_by=popularity.desc&api_key=23442aca5a940bca4f679e9303e5a91e&page=1`
);
const IMG_PATH = "https://image.tmdb.org/t/p/w1280";
const SEARCH_API =
  'https://api.themoviedb.org/3/search/movie?api_key=23442aca5a940bca4f679e9303e5a91e&query="';

const response = await axios.get(API_URL.value);
moviesDB.value = response.data;

const handleSubmit = async (e) => {
  e.preventDefault();
  API_URL.value = SEARCH_API + inputValue.value;
  const response = await axios.get(API_URL.value);
  moviesDB.value = response.data;
};

const colorScore = (score) => {
  if (score >= 8) {
    return "green";
  } else if (score >= 5) {
    return "orange";
  } else {
    return "red";
  }
};
</script>

<template>
  <header>
    <form id="form">
      <input
        type="text"
        id="search"
        class="search"
        placeholder="search"
        v-model="inputValue"
        @keypress.enter="handleSubmit"
      />
    </form>
  </header>

  <main class="main">
    <div class="movie" v-for="movie in moviesDB.results">
      <img :src="`${IMG_PATH}${movie.poster_path}`" alt="poster" />
      <div class="movie-info">
        <h3>{{ movie.original_title }}</h3>
        <span :class="colorScore(movie.vote_average)">
          {{ movie.vote_average }}
        </span>
      </div>
      <div class="overview">
        <h3>Overview</h3>
        {{ movie.overview }}
      </div>
    </div>
  </main>
</template>
