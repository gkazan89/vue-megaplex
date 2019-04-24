<template>
  <div class="home">
    <h1>Welcome to the Megaplex!</h1>
    <h2>Showtimes Available</h2>
    <div class="row">
      <div
        v-for="showtime in showtimes"
        v-bind:showtime="showtime"
        class="col-md-4"
      >
        <div class="card">
          <div class="card-body">
            <p>Movie: {{ showtime.title }}</p>
            <p>Theater: {{ showtime.theater_id }}</p>
            <p>Time: {{ showtime.time }}</p>
            <div v-if="showtime.status.is_sold_out == false">
              <!-- modal -->
              <div>
                <button
                  type="button"
                  class="btn btn-primary"
                  data-toggle="modal"
                  data-target="#exampleModal"
                  v-on:click="currentShowtime = showtime"
                >
                  Buy Ticket
                </button>
              </div>
              <div
                class="modal fade"
                id="exampleModal"
                tabindex="-1"
                role="dialog"
                aria-labelledby="exampleModalLabel"
                aria-hidden="true"
              >
                <div class="modal-dialog" role="document">
                  <div class="modal-content">
                    <div class="modal-header">
                      <h5 class="modal-title" id="exampleModalLabel">
                        Buy ticket for: {{ currentShowtime.title }}
                      </h5>
                      <button
                        type="button"
                        class="close"
                        data-dismiss="modal"
                        aria-label="Close"
                      >
                        <span aria-hidden="true">&times;</span>
                      </button>
                    </div>
                    <div class="modal-body">
                      <div>
                        <ul class="error">
                          <li v-for="error in errors">{{ error }}</li>
                        </ul>
                        </p>
                        <p>name: <input type="text" v-model="name" /></p>
                        <p>email: <input type="text" v-model="email" /></p>
                        <p>
                          credit_card:
                          <input type="number" v-model.number="credit_card" />
                        </p>
                        <p>
                          expiration_date:
                          <input type="date" v-model="expiration_date" />
                        </p>
                      </div>
                    </div>

                    <div class="modal-footer">
                      <button
                        type="button"
                        class="btn btn-secondary"
                        data-dismiss="modal"
                      >
                        Close
                      </button>
                      <button
                        type="button"
                        class="btn btn-primary"
                        v-on:click="buyTicket(currentShowtime)"
                      >
                        Submit
                      </button>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div v-else class="sold_out">
              <p>SOLD OUT</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
.card,
h1 {
  margin: 0.25rem;
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
      errors: [],
      currentShowtime: {},
    };
  },
  created: function() {
    axios.get("http://localhost:3000/api/showtimes").then(
      function(response) {
        this.showtimes = response.data;
      }.bind(this)
    );
  },
  methods: {
    // ticket create
    buyTicket: function(currentShowtime) {
      var self = this;
      this.errors = [];
      var params = {
        name: this.name,
        email: this.email,
        credit_card: this.credit_card,
        expiration_date: this.expiration_date,
        showtime_id: currentShowtime.id
      };
      axios
        .post("http://localhost:3000/api/tickets", params)
        .then(function(response) {
          currentShowtime.name = "";
          currentShowtime.email = "";
          currentShowtime.credit_card = "";
          currentShowtime.expiration_date = "";
          self.$router.go();                  
        })
        .catch(error => {
          console.log(error.response.data.errors);
          this.errors = error.response.data.errors;
        });

    },
  },
  props: ["showtime"]
};
</script>
