<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "This is the movies frontend! Not all movies have directors!",
      movies: [],
      showDirector: false,
    };
  },
  created: function () {
    this.indexMovies();
  },
  methods: {
    indexMovies: function () {
      axios.get("http://localhost:3000/movies").then((response) => {
        console.log(response.data);
        this.movies = response.data;
      });
    },
    createMovie: function () {
      var params = {
        title: this.title,
        year: this.year,
        plot: this.plot,
        director: this.director,
      };
      axios
        .post("http://localhost:3000/movies", params)
        .then((response) => {
          console.log("Here", response.data);
          this.movies.push(response.data);
          this.title = "";
          this.year = "";
          this.plot = "";
          this.director = "";
        })
        .catch((error) => {
          console.log(error.response);
        });
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h1>Create Movie</h1>
    <p>
      Title:
      <input type="text" v-model="title" />
    </p>
    <p>
      Year:
      <input type="text" v-model="year" />
    </p>
    <p>
      Plot:
      <input type="text" v-model="plot" />
    </p>
    <p>
      Director:
      <input type="text" v-model="director" />
    </p>
    <button v-on:click="createMovie()">Create</button>
    <div v-for="movie in movies" v-bind:key="movie.id">
      <h2>Movie: {{ movie.title }}</h2>
      <h4>Year: {{ movie.year }}</h4>
      <h3>Plot: {{ movie.plot }}</h3>
      <button v-on:click="showDirector = !showDirector">Show/Hide Director</button>
      <p v-if="showDirector">{{ movie.director }}</p>
    </div>
  </div>
</template>

<style></style>
