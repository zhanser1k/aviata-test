<template>
  <div class="tickets-list">
    <div class="filters">
      <div class="filters__airlines">
        <dropdown-filter :options="options" @option-click="updateTicketsInfo">
          <template slot="label">
            <span class="filters__dropdown">Авиакомпании</span>
          </template>
        </dropdown-filter>
      </div>
      <div class="filters__only_direct-flights">
        <label>
          <input
            type="checkbox"
            v-model="onlyDirectFlights"
            :value="onlyDirectFlights"
          />
          Только прямые рейсы
        </label>
      </div>
    </div>
    <div class="tickets-list__header">
      <p class="tickets-list_cell tickets-list__header_cell_first">
        Авиакомпания
      </p>
      <p class="tickets-list_cell">Рейс</p>
      <p class="tickets-list_cell_large">Выбор времени</p>
      <p class="tickets-list_cell">Вылет - прилет</p>
      <p class="tickets-list_cell">Время в пути</p>
    </div>
    <transition-group
      name="custom-classes-transition"
      enter-active-class="animated fadeIn"
      leave-active-class="animated fadeOut"
    >
      <ticket
        v-for="(ticket, index) in filteredTickets"
        :ticket="ticket"
        v-bind:key="`ticket-${index}`"
      />
    </transition-group>
  </div>
</template>

<script>
import Ticket from "./Ticket";
import DropdownFilter from "./DropdownFilter";
export default {
  name: "TicketsList",
  data() {
    return {
      options: [
        {
          title: "Air Astana",
          value: "KC"
        },
        {
          title: "Bek Air",
          value: "Z9"
        },
        {
          title: "Scat",
          value: "DV"
        },
        {
          title: "S7 Airlines",
          value: "S7"
        }
      ],
      checkedOptions: [],
      onlyDirectFlights: false
    };
  },
  props: {
    tickets: Array
  },
  components: {
    DropdownFilter,
    Ticket
  },
  computed: {
    filteredTickets() {
      if (!this.checkedOptions.length) {
        return this.tickets;
      }
      return this.tickets.filter(ticket => {
        let result = null;
        ticket.flights.forEach(flight => {
          result = this.checkedOptions.includes(flight.airline.code);
        });
        return result;
      });
    }
  },
  methods: {
    updateTicketsInfo: function(values) {
      this.checkedOptions = values;
    }
  }
};
</script>

<style scoped>
.tickets-list {
  width: 1000px;
  margin: 0 auto;
}

.tickets-list__header {
  display: flex;
  flex-flow: row nowrap;
  justify-content: flex-start;
  width: 100%;
  height: 30px;
  background-color: #397fbf;
  align-items: center;
  color: white;
  font-size: 13px;
}

.tickets-list_cell {
  width: 145px;
}

.tickets-list_cell:nth-child(4) {
  width: 136px;
}

.tickets-list_cell:nth-child(5) {
  width: 90px;
}

.tickets-list__header_cell_first {
  padding-left: 20px;
}

.tickets-list_cell_large {
  width: 178px;
}

.filters {
  display: flex;
  flex-flow: row nowrap;
}

.filters__airlines {
  position: relative;
}

.filters__dropdown:after {
  content: "";
  position: absolute;
  display: block;
  background: url("../assets/images/icons/dropdown-arrow-up.svg") no-repeat;
  transform: rotate(180deg);
  background-size: 11px 6px;
  width: 11px;
  height: 6px;
  top: 5px;
  left: 115px;
}

.filters__only_direct-flights {
  margin-left: 70px;
}
</style>
