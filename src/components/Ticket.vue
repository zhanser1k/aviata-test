<template>
  <div class="ticket">
    <div class="ticket__airline-image ticket_cell ticket_cell_first">
      <img
        v-for="(flight, index) in ticket.flights"
        v-bind:key="`flight-${index}`"
        :src="`../assets/images/${flight.airline.code}.png`"
        alt=""
      />
    </div>
    <div class="ticket__flight ticket_cell">
      {{
        ticket.flights.length === 1
          ? "прямой"
          : `пересадка(${ticket.flights.length - 1})`
      }}
    </div>
    <div class="ticket__select-time ticket_cell_large">
      <span class="ticket_link">Выбрать другое время</span>
      <div class="ticket__share">
        <a class="ticket_link" href="#">Поделиться</a>
      </div>
    </div>
    <div class="ticket__arrival-time ticket_cell" v-html="flightTiming"></div>
    <div class="ticket__duration-time ticket_cell">
      {{ formatFlightDuration(ticket.flightDuration) }}
    </div>
    <div class="ticket__action ticket__action_floating">
      <button class="ticket__action__btn">
        Купить за {{ ticket.price }} KZT
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Ticket",
  props: {
    ticket: Object
  },
  computed: {
    flightTiming: function() {
      const flights = this.ticket.flights;
      return `<span class="ticket__arrival-time_first_time">${
        flights[0].departureTime
      }</span> - <span class="ticket__arrival-time_second-time">${
        flights[flights.length - 1].arrivalTime
      }</span>`;
    }
  },
  methods: {
    formatFlightDuration: function(minutes) {
      return `${Math.trunc(minutes / 60)}ч ${minutes % 60}м`;
    }
  }
};
</script>

<style scoped>
.ticket {
  display: flex;
  flex-flow: row nowrap;
  width: 1000px;
  justify-content: flex-start;
  background: white;
  padding: 15px 20px 55px;
  font-size: 12px;
  line-height: 1.7;
  color: #212c5b;
  box-sizing: border-box;
  border-bottom: 1px solid #dce6ea;
}
.ticket_cell {
  width: 145px;
}
.ticket_cell:nth-child(4) {
  width: 136px;
}
.ticket_cell:nth-child(5) {
  width: 80px;
}
.ticket__airline-image {
  display: flex;
  flex-flow: column wrap;
}
.ticket_link {
  text-decoration: none;
  font-size: 12px;
  text-align: left;
  border-bottom: 1px dashed #9ea3b7;
  color: #212c5b;
}
.ticket_cell_large {
  width: 178px;
}
.ticket__arrival-time_first_time {
  font-weight: bold;
  font-size: 22px;
}
.ticket__action__btn {
  width: 177px;
  display: block;
  background: #fe9922;
  border-radius: 3px;
  border: none;
  text-decoration: none;
  color: #fff;
  font-size: 14px;
  font-weight: normal;
  text-align: center;
  padding: 10px 20px;
  margin: 0 auto;
  cursor: pointer;
}
.ticket__action_floating {
  margin-left: 100px;
}
</style>
