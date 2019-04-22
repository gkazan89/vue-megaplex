<template>
  <div class="home">
    <h1>Owner Portal</h1>
    <ul class="error">
      <li v-for="error in errors">{{ error }}</li>
    </ul>
    <h2>Tickets</h2>
    <p>Filter By Movie: <input type="text" v-model="searchFilter" /></p>
    <div
      v-for="ticket in filterBy(tickets, searchFilter, 'movie')"
      class="ticket"
    >
      <p>Name: {{ ticket.name }}</p>
      <p>Email: {{ ticket.email }}</p>
      <p>Movie: {{ ticket.movie }}</p>
      <p>Showtime: {{ ticket.showtime }}</p>
      <p>Theater: {{ ticket.theater }}</p>
    </div>
    <h3>Movies</h3>
    <div v-for="movie in movies" class="ticket">
      <p>Movie: {{ movie.title }}</p>
      <p>Runtime: {{ movie.runtime }}</p>
      <p>ID: {{ movie.id }}</p>
    </div>
    <div class="ticket">
      <p>Add Movie</p>
      <p>Movie: <input type="text" v-model="title" /></p>
      <p>Runtime (in minutes): <input type="number" v-model="runtime" /></p>
      <button v-on:click="addMovie()">Add Movie</button>
    </div>

    <h3>Theaters</h3>
    <div v-for="theater in theaters" class="ticket">
      <p>Theater ID: {{ theater.theater_id }}</p>
      <p>Capacity: {{ theater.capacity }}</p>
    </div>

    <div class="ticket">
      <p>Add Theater</p>
      <p>Seating Capacity: <input type="number" v-model="capacity" /></p>
      <button v-on:click="addTheat()">Add Theater</button>
    </div>

    <h3>Showtimes</h3>
    <p>Add Showtime</p>
    <p>Movie_ID: <input type="number" v-model="movieID" /></p>
    <p>Theater_ID: <input type="number" v-model="theaterID" /></p>
    <p>Time: <input type="time" v-model="time" /></p>
    <button v-on:click="addShow()">Add Showtime</button>
    <div v-for="showtime in showtimes" class="ticket">
      <p>Movie: {{ showtime.title }}</p>
      <p>Theater: {{ showtime.theater_id }}</p>
      <p>Time: {{ showtime.time }}</p>
    </div>
  </div>
</template>

<style>
.ticket {
  border: solid gray;
  margin-bottom: 1.5rem;
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
    axios.get("http://localhost:3000/api/tickets").then(
      function(response) {
        this.tickets = response.data;
      }.bind(this)
    );

    axios.get("http://localhost:3000/api/movies").then(
      function(response) {
        this.movies = response.data;
      }.bind(this)
    );

    axios.get("http://localhost:3000/api/theaters").then(
      function(response) {
        this.theaters = response.data;
      }.bind(this)
    );
    axios.get("http://localhost:3000/api/showtimes").then(
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
      axios.post("http://localhost:3000/api/movies", params).then(
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
      axios.post("http://localhost:3000/api/theaters", params).then(
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
      axios.post("http://localhost:3000/api/showtimes", params).then(
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
