<template>
  <section id="banner">
    <div class="rides-new">
      <div class="inner">
        <form v-on:submit.prevent="newRide()">
          <h1>Create a ride</h1>
          <ul>
            <li class="text-danger" v-for="error in errors">{{ error }}</li>
          </ul>
          <div class="form-group">
            <label>Name:</label> 
            <input type="text" class="form-control" v-model="rideName">
          </div>
          <div class="form-group">
            <label>Date & Time:</label>
            <datetime
            type="datetime"
            format="MM/dd/yyyy HH:mm"
            v-model="rideDateTime"
            use12-hour auto>
              
            </datetime>
          </div>
          <div class="form-group">
            <label>Start location:</label>
            <input type="text" class="form-control" v-model="rideStart">
          </div>
          <div class="form-group">
            <label>End location:</label>
            <input type="text" class="form-control" v-model="rideEnd">
          </div>
          <div class="form-group">
            <label>Bike type:</label>
            <input type="text" class="form-control" v-model="bikeType">
          </div>
          <br>
          <input type="submit" class="btn btn-primary" value="Submit">
        </form>
      </div>
    </div>
  </section>
</template>

<style>
</style>

<script>
import axios from "axios";
import Vue from 'vue';
import { Datetime } from 'vue-datetime';
Vue.component('datetime', Datetime);

export default {
  data: function() {
    return {
      rideName: "",
      rideDateTime: "",
      rideStart: "",
      rideEnd: "",
      bikeType: "",
      errors: []
    };
  },
  created: function() {},
  methods: {
    newRide: function() {
      var params = {
        name: this.rideName,
        date_time: this.rideDateTime,
        starting_point: this.rideStart,
        end_point: this.rideEnd,
        bike_type: this.bikeType
      };
      axios.post("/api/rides", params).then(response => {
        this.$router.push("/rides");
        console.log(response.data);
      })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    }
  }
};
</script>