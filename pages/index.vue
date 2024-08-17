<template>
  <div class="home">
    <div class="logo">
      <h2>Movies</h2>
    </div>
    
    <form class="search-bar" @submit.prevent="searchMovies">
      <input type="text" placeholder="Qué estás buscando ?" v-model="searchText"/>
      <button type="submit">
        <svg  xmlns="http://www.w3.org/2000/svg"  width="24"  height="24"  viewBox="0 0 24 24"  fill="none"  stroke="currentColor"  stroke-width="2"  stroke-linecap="round"  stroke-linejoin="round"  class="search-icon"><path stroke="none" d="M0 0h24v24H0z" fill="none"/><path d="M10 10m-7 0a7 7 0 1 0 14 0a7 7 0 1 0 -14 0" /><path d="M21 21l-6 -6" /></svg>
      </button>
    </form>

    <MovieList :movies="movies" />
    
  </div>
</template>

<script setup lang="ts" >
import { ref, onMounted } from 'vue';
import env from '../env';
import LocalDB from '../assets/js/LocalDB';

const searchText = ref("");
const movies = ref([]);
const localDB = new LocalDB();

const searchMovies = () => {
  localDB.set('search', searchText.value);
  fetchMovies();
};

const fetchMovies = () => {
  fetch(`https://api.themoviedb.org/3/search/movie?api_key=${env.apikey}&language=en-US&page=1&include_adult=false&query=${localDB.get('search')}`)
    .then(response => response.json())
    .then(data => {
      movies.value = data.results.filter(movie => movie.poster_path !== null && movie.backdrop_path !== null);
      searchText.value = ""; // reset the search field
    });
};

onMounted(() => {
  fetchMovies();
});
</script>


<style lang="scss">
.home {
  // LOGO 
  .logo {
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
    left: 42%;
    width: 250px;
    height: 250px;
    h2 {
      color: white;
      font-size: 60px;
      position: relative;
      top: 15px;
      right: 15px;
    }
    img {
      width: 70%;
      max-width: 500px;
    }
  }
  // SEARCH BAR 
  .search-bar {
    position: relative;
    left: 30%;
    width: 100%;
    margin-top: -20px;
    max-width: 700px;
    background: var(--search-bar-color);
    display: flex;
    align-items: center;
    border-radius: 60px;
    padding: 10px 20px;
    
    input {
      background: transparent;
      flex: 1;
      border: none;
      appearance: none;
      outline: none;
      padding: 24px 20px;
      font-size: 23px;
      color: var(--search-bar-text);
      &::placeholder {
        color: var(--search-bar-placeholder)
      }
    }
    button {
      border: none;
      outline: none;
      text-align: center;
      background: var(--search-icon-background);
      border-radius: 50%;
      width: 60px;
      height: 60px;
      cursor: pointer;
    
      .search-icon {
        width:  27px;
        height: 27px;
        color: var(--search-bar-placeholder);
      }
    }
  }
}
</style>