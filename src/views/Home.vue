<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "This is the movies frontend!",
      movies: [],
      newMovie: {},
      currentMovie: {},
      changeMovie: {},
      showDirector: false,
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
    createMovie: function () {
      axios
        .post("/movies", this.newMovie)
        .then((response) => {
          console.log(response.data);
          this.movies.unshift(response.data);
        })
        .catch((error) => {
          console.log(error.response.data.errors);
        });
      this.newMovie = {};
    },
    showMovie: function (movie) {
      console.log(movie);
      this.currentMovie = movie;
      this.changeMovie = movie;
      document.querySelector("#movie-details").showModal();
    },
    updateMovie: function (movie) {
      axios
        .patch(`/movies/${movie.id}`, movie)
        .then((response) => {
          console.log("Update", response.data);
        })
        .catch((error) => {
          console.log(error.response.data.errors);
        });
    },
    destroyMovie: function (movie) {
      if (confirm("Are you sure about this?")) {
        axios.delete(`/movies/${movie.id}`).then((response) => {
          console.log("Deleted", response.data);
          var index = this.movies.indexOf(movie);
          this.movies.splice(index, 1);
        });
      }
    },
  },
};
</script>

<template>
  <div class="home">
    <h1 style="font-size: 60px">{{ message }}</h1>
    <div v-for="movie in movies" v-bind:key="movie.id">
      <h2 style="color: goldenrod" title="I look like a nice potato chip">Movie: {{ movie.title }}</h2>
      <button v-on:click="showMovie(movie)">Additional Info</button>
    </div>
    <dialog id="movie-details">
      <form method="dialog">
        <h2 style="color: mediumseagreen" title="I look like barf">Movie Info</h2>
        <p>
          Title:
          <input type="text" v-model="changeMovie.title" />
        </p>
        <p>
          Year:
          <input type="text" v-model="changeMovie.year" />
        </p>
        <p>
          Plot:
          <input type="text" v-model="changeMovie.plot" />
        </p>
        <p>
          Director:
          <input type="text" v-model="changeMovie.director" />
        </p>
        <button v-on:click="updateMovie(currentMovie)">Update</button>
        <button v-on:click="destroyMovie(currentMovie)">Destroy</button>
        <button>Close</button>
      </form>
    </dialog>
    <h1>Create Movie</h1>
    <p>{{ newMovie }}</p>
    <p>
      Title:
      <input type="text" v-model="newMovie.title" />
    </p>
    <p>
      Year:
      <input type="text" v-model="newMovie.year" />
    </p>
    <p>
      Plot:
      <input type="text" v-model="newMovie.plot" />
    </p>
    <p>
      Director:
      <input type="text" v-model="newMovie.director" />
    </p>
    <div>
      <button v-on:click="createMovie()">Create</button>
    </div>
    <div>
      <button v-on:click="indexMovies()">Show Movies</button>
    </div>
  </div>
</template>

<style></style>
