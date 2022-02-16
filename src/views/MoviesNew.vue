<script>
import axios from "axios";
export default {
  data: function () {
    return {
      newMovieParams: { title: "", year: "", director: "", plot: "" },
      errors: [],
    };
  },
  methods: {
    createMovie: function () {
      axios
        .post("/movies", this.newMovieParams)
        .then((response) => {
          console.log(response.data);
          // this.movies.unshift(response.data);
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
      this.newMovieParams = {};
    },
  },
};
</script>

<template>
  <div class="movies-new">
    <form v-on:submit.prevent="createMovie()">
      newMovieParams: {{ newMovieParams }}
      <h1>New Movie</h1>
      <ul>
        <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
      </ul>
      <div>
        <label>Title:</label>
        <input type="text" v-model="newMovieParams.title" />
      </div>
      <div>
        <label>Year:</label>
        <input type="text" v-model="newMovieParams.year" />
      </div>
      <div>
        <label>Plot:</label>
        <input type="text" v-model="newMovieParams.plot" />
        <br />
        <small class="danger" v-if="newMovieParams.plot.length < 5">Must be at least 5 characters long!</small>
        <small class="danger" v-if="newMovieParams.plot.length > 340">Must be less than 140 characters!</small>
      </div>
      <div>
        <label>Director:</label>
        <input type="text" v-model="newMovieParams.director" />
      </div>
      <input type="submit" value="Submit" />
    </form>
  </div>
</template>

<style scoped>
.danger {
  color: rgb(171, 45, 45);
}
</style>
