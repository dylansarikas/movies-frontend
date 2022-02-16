<script>
import axios from "axios";
export default {
  data: function () {
    return {
      movies: [],
      titleFilter: "",
    };
  },
  created: function () {
    this.indexMovies();
  },
  methods: {
    indexMovies: function () {
      axios.get("/movies").then((response) => {
        console.log(response.data);
        this.movies = response.data;
      });
    },
  },
  computed: {
    filteredMovies() {
      return this.movies.filter((movie) => {
        return movie.title.toLowerCase().includes(this.titleFilter.toLowerCase());
      });
    },
  },
};
</script>

<template>
  <div class="movies-index">
    <h1>All Movies</h1>
    <p>
      Search:
      <input type="text" v-model="titleFilter" list="movieTitles" />
    </p>
    <datalist id="movieTitles">
      <option v-for="movie in movies" v-bind:key="movie.id">
        {{ movie.title }}
      </option>
    </datalist>
    <transition-group
      appear
      enter-active-class="animate__animated animate__fadeIn"
      leave-active-class="animate__animated animate__fadeOut"
    >
      <div v-for="movie in filteredMovies" v-bind:key="movie.id">
        <router-link v-bind:to="`/movies/${movie.id}`">
          <h2>{{ movie.title }}</h2>
        </router-link>
      </div>
    </transition-group>
  </div>
</template>
