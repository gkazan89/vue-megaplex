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
      searchFilter: ""
    };
  },
  created: function() {
    axios.get("http://localhost:3000/api/tickets").then(
      function(response) {
        console.log(response);
        this.tickets = response.data;
      }.bind(this)
    );
  },
  methods: {},
  props: ["showtime"],
  computed: {}
};
</script>
