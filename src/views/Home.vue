<template>
  <div>
    <v-sheet tile height="54" class="d-flex">
      <v-btn
        icon
        class="ma-2"
        color="grey darken-2"
        @click="$refs.calendar.prev()"
      >
        <v-icon>mdi-chevron-left</v-icon>
      </v-btn>
      <v-select
        v-model="type"
        :items="types"
        dense
        outlined
        hide-details
        class="ma-2"
        label="type"
      ></v-select>
      <v-spacer></v-spacer>
      <v-btn icon class="ma-2" @click="$refs.calendar.next()">
        <v-icon>mdi-chevron-right</v-icon>
      </v-btn>
    </v-sheet>
    <v-sheet height="600">
      <v-calendar
        ref="calendar"
        v-model="value"
        :weekdays="weekday"
        :type="type"
        :event-overlap-mode="mode"
        :event-overlap-threshold="30"
        :events="events"
        :event-color="getEventColor"
        @change="getEvents"
      ></v-calendar>
      <!-- :event-more="false" -->
      <!-- :event-timed="false" -->
    </v-sheet>
  </div>
</template>

<script>
export default {
  name: "Home",
  components: {},
  data() {
    return {
      types: ["month", "week", "day"],
      modes: ["stack", "column"],
      weekday: [0, 1, 2, 3, 4, 5, 6],
      value: "",
      events: [],
      colors: ["cyan"],
      // names: [
      //   "Meeting",
      //   "Holiday",
      //   "PTO",
      //   "Travel",
      //   "Event",
      //   "Birthday",
      //   "Conference",
      //   "Party",
      // ],
    };
  },
  methods: {
    getEvents({ start, end }) {
      const events = [];

      const min = new Date(`${start.date}T00:00:00`);
      const max = new Date(`${end.date}T23:59:59`);
      const days = max.getTime() - min.getTime();
      const eventCount = this.rnd(days, days);

      for (let i = 0; i < eventCount; i++) {
        const allDay = this.rnd(0, 3) === 0;
        // const firstTimestamp = this.rnd(min.getTime(), max.getTime());
        const first = new Date();
        // const secondTimestamp = this.rnd(2, allDay ? 288 : 8) * 900000;
        const second = new Date();

        events.push({
          // name: this.names[this.rnd(0, this.names.length - 1)],
          start: first,
          end: second,
          color: this.colors[this.rnd(0, this.colors.length - 1)],
          timed: allDay,
        });
      }

      this.events = events;
    },
    getEventColor(event) {
      return event.color;
    },
    rnd(a, b) {
      return Math.floor((b - a + 1) * Math.random()) + a;
    },
  },
};
</script>
<style lang="scss" scoped>
::v-deep .theme--light.v-btn.v-btn--has-bg {
  background-color: #7f74b7;
  color: white;
}
</style>
