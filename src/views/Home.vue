<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "This is the movies frontend! Not all movies have directors!",
      movies: [],
      newMovie: {},
      changeMovie: {},
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
      axios
        .post("http://localhost:3000/movies", this.newMovie)
        .then((response) => this.movies.unshift(response.data))
        .catch((error) => {
          console.log(error.response);
        });
      this.newMovie = {};
    },
    updateMovie: function () {
      this.params = {
        title: this.changeMovie.title,
        director: this.changeMovie.director,
        year: this.changeMovie.year,
        plot: this.changeMovie.plot,
      };
      axios
        .patch("http://localhost:3000/movies/" + this.changeMovie.id, this.params)
        .then((response) => console.log(response.data))
        .catch((error) => console.log(error.response));
    },
    destroyMovie: function () {
      axios.delete("http://localhost:3000/movies/" + this.changeMovie.id).then((response) => console.log(response));
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
    <button v-on:click="createMovie()">Create</button>
    <div v-for="movie in movies" v-bind:key="movie.id">
      <h2>Movie: {{ movie.title }}</h2>
      <h4>Year: {{ movie.year }}</h4>
      <h3>Plot: {{ movie.plot }}</h3>
      <button v-on:click="showDirector = !showDirector">Show/Hide Director</button>
      <p v-if="showDirector">{{ movie.director }}</p>
    </div>
    <div>
      <h1>Update Movie</h1>
      <p>
        Id:
        <input type="text" v-model="changeMovie.id" />
      </p>
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
      <button v-on:click="updateMovie()">Commit</button>
    </div>
    <div>
      <h1>Delete Movie</h1>
      <p>
        Id:
        <input type="text" v-model="changeMovie.id" />
      </p>
      <button v-on:click="destroyMovie()">Delete Movie</button>
    </div>
  </div>
</template>

<style></style>
