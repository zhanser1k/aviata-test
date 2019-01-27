<template>
  <div id="app">
    <tickets-list :tickets="tickets" />
  </div>
</template>

<script>
import TicketsList from "./components/TicketsList";
import axios from "axios";

export default {
  name: "app",
  components: {
    TicketsList
  },

  data() {
    return {
      tickets: []
    };
  },

  created() {
    axios
      .get("./data.json")
      .then(response => {
        this.tickets = response.data;
      })
      .catch(error => {
        throw error;
      });
  }
};
</script>

<style>
@import "~animate.css";
@import "./assets/css/fonts.css";

body {
  background: #dff0ff;
  overflow-y: scroll;
}

#app {
  font-family: "Open Sans", Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #212c5b;
  margin-top: 170px;
}
</style>
