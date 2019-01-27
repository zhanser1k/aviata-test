<template>
  <div class="tickets-list">
    <div class="filters">
      <div class="filters__airlines">
        <dropdown-filter
          :options="filterOptions"
          @option-click="updateTicketsInfo"
        >
          <template slot="label">
            Авиакомпании
          </template>
        </dropdown-filter>
      </div>
      <div class="filters__only_direct">
        <label class="filters__only-direct_label">
          <input
            type="checkbox"
            class="filters__checkbox"
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
      name="tickets-list-transition"
      enter-active-class="animated fadeIn"
      leave-active-class="animated fadeOut"
      v-if="getOnlyDirectFlights"
    >
      <ticket
        v-for="(ticket, index) in getOnlyDirectFlights"
        :ticket="ticket"
        v-bind:key="`ticket-${index}`"
      />
    </transition-group>
    <transition
      name="no-results-text-transition"
      enter-active-class="animated fadeIn"
      leave-active-class="animated fadeOut"
      v-if="!getOnlyDirectFlights"
    >
      <div class="no-results">
        <p class="no-results__text">Нет результатов</p>
      </div>
    </transition>
  </div>
</template>

<script>
import Ticket from "./Ticket";
import DropdownFilter from "./DropdownFilter";
export default {
  name: "TicketsList",
  data() {
    return {
      filterOptions: [
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
    tickets: {
      type: Array,
      required: true
    }
  },
  components: {
    DropdownFilter,
    Ticket
  },
  computed: {
    filteredTickets() {
      if (!this.checkedOptions.length) {
        return;
      }
      return this.tickets.filter(ticket => {
        let result = null;
        ticket.flights.forEach(flight => {
          result = this.checkedOptions.includes(flight.airline.code);
        });
        return result;
      });
    },
    getOnlyDirectFlights() {
      if (this.onlyDirectFlights) {
        return this.filteredTickets.filter(ticket => {
          return ticket.flights.length === 1;
        });
      } else {
        return this.filteredTickets;
      }
    }
  },
  methods: {
    updateTicketsInfo(values) {
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
  justify-content: flex-start;
  align-items: center;
}

.filters__airlines {
  position: relative;
}

.filters__only_direct {
  margin-left: 70px;
}

.filters__checkbox {
  cursor: pointer;
  display: inline-block;
  vertical-align: middle;
  -webkit-appearance: none;
  background-color: transparent;
  border: 1px solid #212c5b;
  padding: 9px;
  border-radius: 5px;
  position: relative;
}

.filters__checkbox:checked {
  background-color: #212c5b;
  color: #ffffff;
}

.filters__checkbox:checked:after {
  content: "";
  position: absolute;
  top: 5px;
  left: 2px;
  background: url("../assets/images/icons/check.svg") no-repeat;
  background-size: 15px 10px;
  width: 15px;
  height: 10px;
}

.filters__checkbox:focus {
  outline: 0;
}

.filters__airlines {
  font-size: 14px;
}

.filters__only-direct_label {
  cursor: pointer;
  font-size: 14px;
}
.no-results {
  background: white;
  height: 100px;
  width: 100%;
}
.no-results__text {
  font-size: 22px;
  font-weight: 300;
  text-align: center;
  margin: 0;
  padding: 30px 0 30px;
}
</style>
