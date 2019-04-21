<template>
  <div class="home">
    <h1>Showtimes Available</h1>
    <div v-for="showtime in showtimes" class="showtime">
      <p>Movie: {{ showtime.title }}</p>
      <p>Theater: {{ showtime.theater_id }}</p>
      <p>Time: {{ showtime.time }}</p>
      <p>
        Soldout?
        {{
          showtime.status.is_sold_out
            | moment("timezone", "America/Los_Angeles", "LLLL ss")
        }}
      </p>
    </div>
  </div>
</template>

<style>
.showtime {
  border: solid gray;
  margin-bottom: 1.5rem;
}
</style>

<script>
var axios = require("axios");

export default {
  data: function() {
    return {
      message: "Welcome to My Site!",
      showtimes: []
    };
  },
  created: function() {
    axios.get("http://localhost:3000/api/showtimes").then(
      function(response) {
        console.log(response);
        this.showtimes = response.data;
      }.bind(this)
    );
  },
  methods: {},
  computed: {}
};
</script>
