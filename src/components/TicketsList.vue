<template>
  <div class="tickets-list">
    <div class="filters"></div>
    <div class="tickets-list__header">
      <p class="tickets-list_cell tickets-list__header_cell_first">
        Авиакомпания
      </p>
      <p class="tickets-list_cell">Рейс</p>
      <p class="tickets-list_cell_large">Выбор времени</p>
      <p class="tickets-list_cell">Вылет - прилет</p>
      <p class="tickets-list_cell">Время в пути</p>
    </div>
    <ticket
      v-for="(ticket, index) in tickets"
      :ticket="ticket"
      v-bind:key="`ticket-${index}`"
    />
  </div>
</template>

<script>
import Ticket from "./Ticket.vue";
export default {
  name: "TicketsList",
  props: {
    tickets: Array
  },
  components: {
    Ticket
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
</style>
