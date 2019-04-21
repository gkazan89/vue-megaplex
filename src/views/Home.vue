<template>
  <div class="home">
    <h1>Showtimes Available</h1>
    <ul class="error">
      <li v-for="error in errors">{{ error }}</li>
    </ul>
    <div
      v-for="showtime in showtimes"
      v-bind:showtime="showtime"
      class="showtime"
    >
      <p>Movie: {{ showtime.title }}</p>
      <p>Theater: {{ showtime.theater_id }}</p>
      <p>Time: {{ showtime.time }}</p>
      <p>
        Soldout?
        {{ showtime.status.is_sold_out }}
      </p>
      <div v-if="showtime.status.is_sold_out == false">
        <button v-on:click="buyTicket(showtime)">BUY!</button>
        <p>Buy Ticket</p>
        <div>
          <p>name: <input type="text" v-model="name" /></p>
          <p>email: <input type="text" v-model="email" /></p>
          <p>
            credit_card: <input type="number" v-model.number="credit_card" />
          </p>
          <p>
            expiration_date: <input type="date" v-model="expiration_date" />
          </p>
          <p>{{ showtime.id }}</p>
        </div>
        <!--         name
        email
        credit_card
        expiration_date
        showtime_id -->
      </div>
      <div v-else class="sold_out">
        <p>SOLD OUT</p>
      </div>
    </div>
  </div>
</template>

<style>
.showtime {
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

export default {
  data: function() {
    return {
      message: "Welcome to My Site!",
      showtimes: [],
      name: "",
      email: "",
      credit_card: "",
      expiration_date: "",
      errors: []
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
  methods: {
    // ticket create
    buyTicket: function(showtime) {
      this.errors = [];
      var params = {
        name: this.name,
        email: this.email,
        credit_card: this.credit_card,
        expiration_date: this.expiration_date,
        showtime_id: showtime.id
      };
      axios
        .post("http://localhost:3000/api/tickets", params)
        .then(function(response) {
          console.log("RESPONSE: ", response);
        })
        .catch(error => {
          console.log(error.response.data.errors);
          this.errors = error.response.data.errors;
        });
    }
  },
  props: ["showtime"],
  computed: {}
};
</script>
