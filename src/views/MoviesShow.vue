<script>
import axios from "axios";
export default {
  data: function () {
    return {
      movie: {},
    };
  },
  created: function () {
    axios.get(`/movies/${this.$route.params.id}`).then((response) => {
      console.log(response.data);
      this.movie = response.data;
    });
  },
  methods: {
    destroymovie: function () {
      if (confirm("Are you sure about that?")) {
        axios.delete(`/movies/${this.movie.id}`).then((response) => {
          console.log(response.data);
          this.$router.push("/movies");
        });
      }
    },
  },
};
</script>

<template>
  <div class="movies-show">
    <h2>{{ movie.title }}</h2>
    <h2>{{ movie.year }}</h2>
    <h2>{{ movie.plot }}</h2>
    <h2>{{ movie.director }}</h2>
    <div>
      <button><router-link v-bind:to="`/movies/${movie.id}/edit`">Edit</router-link></button>
      |
      <button v-on:click="destroymovie()">Delete</button>
    </div>
  </div>
</template>
