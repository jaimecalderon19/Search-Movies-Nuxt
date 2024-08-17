<template>
    <div class="movie-banner">
        <a @click="$router.go(-1)">
            <svg  xmlns="http://www.w3.org/2000/svg"  width="24"  height="24"  viewBox="0 0 24 24"  fill="none"  stroke="currentColor"  stroke-width="2"  stroke-linecap="round"  stroke-linejoin="round"  class="back-button"><path stroke="none" d="M0 0h24v24H0z" fill="none"/><path d="M5 12l14 0" /><path d="M5 12l6 6" /><path d="M5 12l6 -6" /></svg>
        </a>
        <img :src="`https://image.tmdb.org/t/p/original/${movie.backdrop_path}`" :alt="movie.title + ' poster'" class="movie-backdrop"/>
        <div class="movie-poster">
            <img :src="`https://image.tmdb.org/t/p/original/${movie.poster_path}`" :alt="movie.title + ' poster'"/>
        </div>
        <div class="movie-detail">
            <div class="movie-text">
                <h2>{{ movie.title }}</h2>
                <h4>{{ movie.tagline }}</h4>
                <div v-for="genre in genres" :key="genre.id" class="movie-genres">
                    <span v-if="genres.indexOf(genre) !== genres.length - 1" class="genre-name"><i>{{ genre.name }}, </i></span>
                    <span v-else class="genre-name"><i>{{ genre.name }}</i></span>
                </div>
                <div class="movie-datetime">
                    <span>
                        <svg  xmlns="http://www.w3.org/2000/svg"  width="24"  height="24"  viewBox="0 0 24 24"  fill="none"  stroke="currentColor"  stroke-width="2"  stroke-linecap="round"  stroke-linejoin="round"  class="icon"><path stroke="none" d="M0 0h24v24H0z" fill="none"/><path d="M3 12a9 9 0 1 0 18 0a9 9 0 0 0 -18 0" /><path d="M12 7v5l3 3" /></svg>
                        {{ movie.runtime }} mins
                    </span>
                    <span>
                        <svg  xmlns="http://www.w3.org/2000/svg"  width="24"  height="24"  viewBox="0 0 24 24"  fill="none"  stroke="currentColor"  stroke-width="2"  stroke-linecap="round"  stroke-linejoin="round"  class="icon"><path stroke="none" d="M0 0h24v24H0z" fill="none"/><path d="M4 7a2 2 0 0 1 2 -2h12a2 2 0 0 1 2 2v12a2 2 0 0 1 -2 2h-12a2 2 0 0 1 -2 -2v-12z" /><path d="M16 3v4" /><path d="M8 3v4" /><path d="M4 11h16" /><path d="M11 15h1" /><path d="M12 15v3" /></svg>
                        {{ year }}
                    </span>
                    <span>
                        <svg  xmlns="http://www.w3.org/2000/svg"  width="24"  height="24"  viewBox="0 0 24 24"  fill="none"  stroke="currentColor"  stroke-width="2"  stroke-linecap="round"  stroke-linejoin="round"  class="icon"><path stroke="none" d="M0 0h24v24H0z" fill="none"/><path d="M12 17.75l-6.172 3.245l1.179 -6.873l-5 -4.867l6.9 -1l3.086 -6.253l3.086 6.253l6.9 1l-5 4.867l1.179 6.873z" /></svg>
                        {{ rating }}
                    </span>
                </div>
                
                <p>{{ movie.overview }}</p>
            </div>
        </div>
    </div>

    <div class="movie-detail">

    </div>
</template>
<script setup lang="ts">
import { ref, onBeforeMount } from 'vue';
import env from '../../env';
import '@/assets/styles/variables.css';
import { useRoute } from 'nuxt/app';

const route = useRoute()
const movie = ref({});
const genres = ref([]);
const year = ref("");
const rating = ref("");

onBeforeMount(() => {
  fetch(`https://api.themoviedb.org/3/movie/${route.params.id}?api_key=${env.apikey}&language=en-US`)
    .then(response => response.json())
    .then(data => {
      movie.value = data;  // returns a Movie object
      genres.value = movie.value.genres;
      year.value = movie.value.release_date.slice(0, 4);
      rating.value = movie.value.vote_average.toFixed(1);
    });
});
</script>


<style lang="scss">

    .back-button {
        color: black;
        font-size: 40px;
        position: absolute;
        top: 10px;
        left: 10px;
        cursor: pointer;
    }

    .back-button:hover {
        color: white;
    }

    .movie-banner {
        position: relative;
        width: 100%;
        min-height: 91vh;
        display: flex;
        justify-content: center;
        align-items: center;
        background: linear-gradient(to right, rgba(181, 180, 180, 0.2), 30%, rgba(0, 0, 0, 0.7));

        .movie-backdrop {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            object-fit: cover;
            z-index: -1;
        }

        .movie-detail {
            display: flex;
            justify-content: flex-start;
            width: 80%;

            .movie-text {
                color: white;
                margin-left: 40px;
                height: 100%;
                position: relative;
                bottom: 60px;

                h2 {
                    font-size: 60px;
                    font-weight: 600;
                    margin-bottom: 7px;
                    max-width: 60%;
                    line-height: 100%;
                }

                h4 {
                    font-size: 20px;
                    font-weight: 500;
                    margin-bottom: 15px;
                    letter-spacing: 2px;
                }

                .movie-genres {
                    display: inline;

                    .genre-name {
                        font-size: 17px;
                        font-weight: 300;
                    }
                }

                .movie-datetime {
                    margin-top: 5px;
                    margin-bottom: 15px;
                    font-size: 14px;
                    font-weight: 300;
                    display: flex;
                    column-gap: 15px;

                    span {
                        .icon {
                            margin-right: 4px;
                            width: 14px;
                            height: 14px;
                            position: relative;
                            top: 2px;
                        }
                    }
                }

                p {
                    max-width: 50%;
                    font-weight: 200;
                    font-size: 16px;
                    letter-spacing: 1px;
                }
            }
        }

        .movie-poster {
            position: relative;
            width: 400px;
            height: auto;
            margin-left: 120px;

            img {
                width: 100%;
                border-radius: 20px;
            }
        }
    }

</style>