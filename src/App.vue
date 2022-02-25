<script>
export default {
  data: function () {
    return {
      isLoggedIn: !!localStorage.jwt,
    };
  },
  watch: {
    $route: function () {
      this.isLoggedIn = !!localStorage.jwt;
    },
  },
  methods: {
    // isLoggedIn: function () {
    //   return localStorage.getItem("jwt");
    // }
  },
};
</script>

<template>
  <nav class="navbar navbar-expand-lg navbar-light bg-light">
    <div class="container-fluid">
      <router-link class="navbar-brand" to="/">Navbar</router-link>
      <button
        class="navbar-toggler"
        type="button"
        data-bs-toggle="collapse"
        data-bs-target="#navbarNavAltMarkup"
        aria-controls="navbarNavAltMarkup"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarNavAltMarkup">
        <div class="navbar-nav">
          <router-link class="nav-link active" aria-current="page" to="/">Home</router-link>
          <router-link v-if="!isLoggedIn" class="nav-link" to="/signup">Signup</router-link>
          <router-link v-if="!isLoggedIn" class="nav-link" to="/login">Login</router-link>
          <router-link v-if="isLoggedIn" class="nav-link" to="/logout">Logout</router-link>
          <router-link class="nav-link" to="/movies">Movie Index</router-link>
          <router-link v-if="isLoggedIn" class="nav-link" to="/movies/new">New Movie</router-link>
        </div>
      </div>
      <!-- <form class="form-inline">
        <div class="input-group">
          <div class="input-group-prepend">
            <span class="input-group-text" id="basic-addon1">#</span>
          </div>
          <input
            type="text"
            v-model="titleFilter"
            list="postTitles"
            placeholder="Blog Title"
            aria-label="Username"
            aria-describedby="basic-addon1"
          />
        </div>
      </form> -->
    </div>
  </nav>
  <div class="container">
    <router-view />
  </div>

  <!-- <div id="nav">
    <router-link to="/">Home</router-link>
    |
    <span v-if="isLoggedIn">
      <router-link to="/logout">Logout</router-link>
    </span>
    <span v-else>
      <router-link to="/signup">Signup</router-link>
      |
      <router-link to="/login">Login</router-link>
    </span>
    |
    <router-link to="/movies">Movie Index</router-link>
    |
    <router-link to="/movies/new">New Movie</router-link>
  </div>
  <router-view /> -->
</template>
