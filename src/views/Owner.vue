<template>
  <div class="home col">
    <div>
      <h1>Owner Portal</h1>
      <ul class="error">
        <li v-for="error in errors">{{ error }}</li>
      </ul>

      <!-- tickets -->
      <h2>Tickets</h2>
      <p class="above">
        Filter By Movie: <input type="text" v-model="searchFilter" />
      </p>
      <div class="row">
        <div
          v-for="ticket in filterBy(tickets, searchFilter, 'movie')"
          class="col-md-4"
        >
          <div class="card">
            <p>Name: {{ ticket.name }}</p>
            <p>Email: {{ ticket.email }}</p>
            <p>Movie: {{ ticket.movie }}</p>
            <p>Showtime: {{ ticket.showtime }}</p>
            <p>Theater: {{ ticket.theater }}</p>
          </div>
        </div>
      </div>

      <!-- movies -->
      <h3>Movies</h3>
      <div class="row">
        <div class="col-md-4">
          <div class="card">
            <p>Add Movie</p>
            <p>Movie: <input type="text" v-model="title" /></p>
            <p>
              Runtime (in minutes): <input type="number" v-model="runtime" />
            </p>
            <button class="btn btn-primary" v-on:click="addMovie()">
              Add Movie
            </button>
          </div>
        </div>
      </div>
      <div class="row">
        <div v-for="movie in movies" class="col-md-4">
          <div class="card">
            <p>Movie: {{ movie.title }}</p>
            <p>Runtime: {{ movie.runtime }} minutes</p>
            <p>ID: {{ movie.id }}</p>
          </div>
        </div>
      </div>

      <!-- theaters -->
      <h3>Theaters</h3>
      <div class="row">
        <div class="col-md-4">
          <div class="card">
            <p>Add Theater</p>
            <p>Seating Capacity: <input type="number" v-model="capacity" /></p>
            <button class="btn btn-primary" v-on:click="addTheat()">
              Add Theater
            </button>
          </div>
        </div>
      </div>

      <div class="row">
        <div v-for="theater in theaters" class="col-md-4">
          <div class="card">
            <p>Theater ID: {{ theater.theater_id }}</p>
            <p>Capacity: {{ theater.capacity }}</p>
          </div>
        </div>
      </div>

      <!-- showtimes -->
      <h3>Showtimes</h3>
      <div class="row">
        <div class="col-md-4">
          <div class="card">
            <p>Add Showtime</p>
            <p>Movie_ID: <input type="number" v-model="movieID" /></p>
            <p>Theater_ID: <input type="number" v-model="theaterID" /></p>
            <p>Time: <input type="time" v-model="time" /></p>
            <button class="btn btn-primary" v-on:click="addShow()">
              Add Showtime
            </button>
          </div>
        </div>
      </div>
      <div class="row">
        <div v-for="showtime in showtimes" class="col-md-4">
          <div class="card">
            <p>Movie: {{ showtime.title }}</p>
            <p>Theater: {{ showtime.theater_id }}</p>
            <p>Time: {{ showtime.time }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
h3,
#film {
  margin: 0.5rem 1rem;
}

.sold_out,
.error {
  color: red;
}

input {
  border: 0.025rem solid black;
}
</style>

<script>
var axios = require("axios");

import Vue2Filters from "vue2-filters";

export default {
  mixins: [Vue2Filters.mixin],
  data: function() {
    return {
      tickets: [],
      errors: [],
      searchFilter: "",
      movies: [],
      theaters: [],
      showtimes: [],
      title: "",
      runtime: "",
      capacity: "",
      movieID: "",
      theaterID: "",
      time: ""
    };
  },
  created: function() {
    axios.get("/api/tickets").then(
      function(response) {
        this.tickets = response.data;
      }.bind(this)
    );

    axios.get("/api/movies").then(
      function(response) {
        this.movies = response.data;
      }.bind(this)
    );

    axios.get("/api/theaters").then(
      function(response) {
        this.theaters = response.data;
      }.bind(this)
    );
    axios.get("/api/showtimes").then(
      function(response) {
        this.showtimes = response.data;
      }.bind(this)
    );
  },
  methods: {
    addMovie: function() {
      var params = {
        name: this.title,
        runtime: this.runtime
      };
      axios.post("/api/movies", params).then(
        function(response) {
          this.movies.push(response.data);
        }.bind(this)
      );
      this.title = "";
      this.runtime = "";
    },

    addTheat: function() {
      var params = {
        capacity: this.capacity
      };
      axios.post("/api/theaters", params).then(
        function(response) {
          this.theaters.push(response.data);
        }.bind(this)
      );
      this.capacity = "";
    },

    addShow: function() {
      var params = {
        movie_id: this.movieID,
        theater_id: this.theaterID,
        time: this.time
      };
      axios.post("/api/showtimes", params).then(
        function(response) {
          this.showtimes.push(response.data);
        }.bind(this)
      );
      this.movieID = "";
      this.theaterID = "";
      this.time = "";
    }
  },
  props: ["showtime"],
  computed: {}
};
</script>
