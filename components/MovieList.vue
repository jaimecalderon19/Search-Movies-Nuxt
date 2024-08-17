<template>
    <div class="movie-list">
      <div class="movie-card" v-for="movie in movies" :key="movie.id">
        <router-link :to="'/movie/' + movie.id" class="movie-link">
          <div class="movie-poster">
            <img :src="`https://image.tmdb.org/t/p/original/${movie.backdrop_path}`" :alt="movie.title + ' poster'" />
          </div>
          <div class="movie-detail">
            <h3>{{ movie.title }}</h3>
            <div class="movie-ratings">
              <span>{{ movie.vote_average }}</span>
              <svg  xmlns="http://www.w3.org/2000/svg"  width="24"  height="24"  viewBox="0 0 24 24"  fill="#F9E400"  class="icon star-icon"><path stroke="none" d="M0 0h24v24H0z" fill="none"/><path d="M8.243 7.34l-6.38 .925l-.113 .023a1 1 0 0 0 -.44 1.684l4.622 4.499l-1.09 6.355l-.013 .11a1 1 0 0 0 1.464 .944l5.706 -3l5.693 3l.1 .046a1 1 0 0 0 1.352 -1.1l-1.091 -6.355l4.624 -4.5l.078 -.085a1 1 0 0 0 -.633 -1.62l-6.38 -.926l-2.852 -5.78a1 1 0 0 0 -1.794 0l-2.853 5.78z" /></svg>
              <!-- <uis-star class="star-icon"/> -->
            </div>
            <p v-if="movie.overview !== ''">{{ movie.overview }}</p>
            <p v-else>Click to see more details</p>
          </div>
        </router-link>
      </div>
    </div>
</template>

<script setup lang="ts">
// import { UisStar } from '@iconscout/vue-unicons-solid';

// Definimos las props
const props = defineProps(['movies']);
</script>


<style lang="scss">
// MOVIE 
  .movie-list {
    display: flex;
    flex-wrap: wrap;
    margin-top: 80px;
    max-height: 75%;
    overflow-y: scroll;
    -ms-overflow-style: none;
    scrollbar-width: none;
    &::-webkit-scrollbar {
      display: none;
    }
    .movie-card {
      flex: 1 1 33.33%;
      position: relative;
      background-color: transparent;
      border-radius: 20px;
      overflow: hidden;
      .movie-poster {
        position: relative;
        overflow: hidden;
        img {
          width: 100%;
          transition: 0.5s;
        }
        &::before {
          content: '';
          position: absolute;
          bottom: -170px;
          width: 100%;
          height: 100%;
          background: linear-gradient(0deg, var(--card-overlay-color) 50%, transparent);
          transition: 0.5s;
          z-index: 1;
        }
      }
      &:hover {
        .movie-poster::before {
          bottom: 0px;
        }
        .movie-poster {
          img {
            transform: translateY(-50px);
            filter: blur(3px);
          }
        }
        .movie-detail {
          bottom: 8px;
        }
      }
      .movie-detail {
        position: absolute;
        bottom: -78px;
        left: 0;
        padding: 20px;
        width: 100%;
        z-index: 2;
        transition: 0.5s;
        h3 {
          color: white;
          font-weight: 500;
          font-size: 28px
        }
        .movie-ratings {
          margin-bottom: 15px;
        }
        span {
          color: var(--movie-card-text-color);
          font-weight: 400;
          font-size: 18px;
        }
        
        p {
          color: var(--movie-card-text-color);
          font-weight: 200;
          font-size: 14px;
          max-width: 100%;
          white-space: nowrap;
          overflow: hidden;
          text-overflow: ellipsis;
        }
        .star-icon {
          width: 20px;
          height: 20px;
          margin-left: 4px;
          padding-top: 2.3px;
        }
      }
    }
  }
</style>