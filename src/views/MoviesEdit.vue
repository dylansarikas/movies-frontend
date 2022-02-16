<script>
import axios from "axios";
export default {
  data: function () {
    return {
      movie: {},
      errors: [],
    };
  },
  created: function () {
    axios.get(`/movies/${this.$route.params.id}`).then((response) => {
      console.log(response.data);
      this.movie = response.data;
    });
  },
  methods: {
    updateMovie: function () {
      axios
        .patch(`/movies/${this.movie.id}`, this.movie)
        .then((response) => {
          console.log("Edit movie:", response.data);
          this.$router.push(`/movies/${this.movie.id}`);
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>

<template>
  <div class="movies-new">
    <form v-on:submit.prevent="updateMovie()">
      <h1>Edit movie</h1>
      <ul>
        <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
      </ul>
      <p>Current movie: {{ movie }}</p>
      <div>
        <label>Title:</label>
        <input type="text" v-model="movie.title" />
      </div>
      <div>
        <label>Year:</label>
        <input type="text" v-model="movie.year" />
      </div>
      <div>
        <label>Plot:</label>
        <input type="text" v-model="movie.plot" />
      </div>
      <div>
        <label>Director:</label>
        <input type="text" v-model="movie.director" />
      </div>
      <input type="submit" value="Submit" />
    </form>
  </div>
</template>
