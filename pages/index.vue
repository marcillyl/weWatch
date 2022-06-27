<template>
  <div>
    <Hero />
    <div class="search">
      <input
        @keyup.enter="$fetch"
        type="text"
        placeholder="Search"
        v-model.lazy="searchInput"
      />
      <button
        @click="clearSearch"
        v-show="searchInput != ''"
        class="search__button"
      >
        Clear Search
      </button>
    </div>
    <div v-if="searchInput !== ''">
      <div id="movies" class="movies">
        <div
          v-for="(movie, index) in searchedMovies"
          class="movie"
          :key="index"
        >
          <div>
            <img
              :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`"
              alt=""
              class="movie__image"
            />
            <p class="movie__review">{{ movie.vote_average }}</p>
          </div>
          <div>
            <p class="movie__title">
              {{ movie.title.slice(0, 20) }}
              <span v-if="movie.title.length > 20">...</span>
            </p>
            <p class="movie__release">
              Released :
              {{
                new Date(movie.release_date).toLocaleString(`en-us`, {
                  month: "long",
                  day: "numeric",
                  year: "numeric",
                })
              }}
            </p>
            <NuxtLink
              class="movie__link"
              :to="{ name: 'movies-movieid', params: { movieid: movie.id } }"
            >
              Get more info
            </NuxtLink>
          </div>
        </div>
      </div>
    </div>
    <div v-else>
      <div id="movies" class="movies">
        <div v-for="(movie, index) in movies" class="movie" :key="index">
          <div>
            <img
              :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`"
              alt=""
              class="movie__image"
            />
            <p class="movie__review">{{ movie.vote_average }}</p>
            <p class="movie__overview"></p>
          </div>
          <div>
            <p class="movie__title">
              {{ movie.title.slice(0, 20) }}
              <span v-if="movie.title.length > 20">...</span>
            </p>
            <p class="movie__release">
              Released :
              {{
                new Date(movie.release_date).toLocaleString(`en-us`, {
                  month: "long",
                  day: "numeric",
                  year: "numeric",
                })
              }}
            </p>
            <NuxtLink
              class="movie__link"
              :to="{ name: 'movies-movieid', params: { movieid: movie.id } }"
            >
              Get more info
            </NuxtLink>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "IndexPage",
  data() {
    return {
      movies: [],
      searchedMovies: [],
      searchInput: "",
    };
  },
  async fetch() {
    if (this.searchInput == "") {
      await this.getMovie();
      return;
    }
    await this.searchMovies();
  },
  methods: {
    async getMovie() {
      const data = axios.get(
        `https://api.themoviedb.org/3/movie/now_playing?api_key=37ed43a4f8eaa2abd75f9283692947bc&language=en-US&page=1`
      );
      const result = await data;
      result.data.results.forEach((movie) => {
        this.movies.push(movie);
      });
    },
    async searchMovies() {
      const data = axios.get(
        `https://api.themoviedb.org/3/search/movie?api_key=37ed43a4f8eaa2abd75f9283692947bc&language=en-US&page=1&query=${this.searchInput}`
      );
      const result = await data;
      result.data.results.forEach((movie) => {
        this.searchedMovies.push(movie);
      });
    },
    clearSearch() {
      this.searchInput = "";
      this.searchedMovies = [];
    },
  },
};
</script>

<style scoped>
.movies {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
}
.movie {
  max-width: 300pt;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  padding: 10pt;
  margin: 17pt;
  background: #ffffff11;
  border: solid thin #ffffff22;
}
.movie__image {
  width: 170pt;
  height: 240pt;
  object-fit: cover;
  margin-bottom: 10pt;
}
.movie__overview {
  margin: 10pt;
}
.movie__title {
  font-size: 1.1em;
  margin-bottom: 10pt;
  color: aqua;
}
.movie__release {
  margin-bottom: 10pt;
}
.movie__link {
  font-size: 1.4em;
  font-weight: 700;
  color: crimson;
  padding: 7pt 10pt;
}
.search {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 17pt 10pt;
}
.search__button {
  padding: 7pt 10pt;
  background-color: crimson;
  color: white;
}
input {
  max-width: 240pt;
  width: 100%;
  padding: 10pt 7pt;
  margin: 10pt 0;
  border: solid thin #ffffff22;
  background: #ffffff11;
}
input:focus {
  outline: none;
}
.button {
  border-top-left-radius: 0;
  border-bottom-left-radius: 0;
}
</style>
