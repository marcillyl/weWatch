<template>
  <div class="container">
    <Loading v-if="$fetchState.pending" />
    <div class="container single-movie">
      <NuxtLink :to="{ name: 'index' }" class="container__link">Back</NuxtLink>
      <div class="movie">
        <img
          :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`"
          alt=""
          class="movie__image"
        />
        <h1 class="movie__headline">{{ movie.title }}</h1>
        <p class="movie__text">{{ movie.overview }}</p>
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
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "single-movie",
  data() {
    return {
      movie: "",
    };
  },
  async fetch() {
    await this.gatSingleMovie();
  },
  methods: {
    async gatSingleMovie() {
      const data = axios.get(
        `https://api.themoviedb.org/3/movie/${this.$route.params.movieid}?api_key=37ed43a4f8eaa2abd75f9283692947bc&language=en-US`
      );
      const result = await data;
      this.movie = result.data;
    },
  },
};
</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.container__link {
  margin: 10pt;
  font-size: 1.4em;
  font-weight: 900;
  padding: 7pt 10pt;
  background: crimson;
}
.movie {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  max-width: 400pt;
}
.movie__image {
  max-width: 300pt;
  max-height: 400pt;
}
.movie__headline {
  font-size: 4em;
  font-weight: 900;
  text-align: center;
  max-width: 400pt;
}
.movie__text {
  font-size: 1.1em;
  line-height: 1.7em;
  text-align: center;
  margin: 10pt;
}
.movie__release {
  font-size: 1.4em;
  font-weight: 700;
  margin: 10pt;
}
</style>
