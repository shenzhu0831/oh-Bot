<template>
  <div class="Calendar">
    <div class="month_calendar"></div>
    <div class="weekly_calendar">
      <!-- <pre>{{ events }}</pre> -->
      <v-btn
        icon
        class="ma-2"
        color="grey darken-2"
        @click="$refs.calendar.prev()"
      >
        <v-icon>mdi-chevron-left</v-icon>
      </v-btn>
      <v-btn icon class="ma-2" @click="$refs.calendar.next()">
        <v-icon>mdi-chevron-left</v-icon>
      </v-btn>
      <v-sheet tile height="54" class="navbar">
        <h1 class="calendar_title">預約行事曆</h1>
        <v-select
          class="change_week_and_day"
          :value="type"
          :items="types"
          @change="changeWeekAndDay($event)"
          dense
          background-color="#dedaf4"
        ></v-select>
        <v-select
          class="user"
          :value="user"
          :items="users"
          @change="changeUser($event)"
          dense
          background-color="#dedaf4"
        ></v-select>
      </v-sheet>
      <v-sheet class="calendar_header">
        <div class="week_day" v-for="(item, index) in dayText" :key="index">
          <div
            class="week_text"
            :class="{ active: item.day === isToday ? true : false }"
          >
            <span class="week_text_english">{{ item.text }}</span>
            <span class="week_text_number" v-if="isShow">
              {{ item.day }}
            </span>
          </div>
        </div>
      </v-sheet>
      <v-sheet>
        <v-calendar
          ref="calendar"
          v-model="value"
          :weekdays="weekday"
          :type="type"
          :events="events"
          :event-more="false"
          :event-timed="'false'"
          :event-overlap-mode="mode"
          :event-overlap-threshold="30"
          :event-color="color"
          :hide-header="true"
          @change="getEvents"
        ></v-calendar>
      </v-sheet>
    </div>
  </div>
</template>

<script>
import dayjs from "dayjs";
export default {
  name: "Home",
  components: {},
  data() {
    return {
      type: "week",
      types: ["week", "day"],
      mode: "stack",
      modes: ["stack", "column"],
      weekday: [1, 2, 3, 4, 5, 6, 0],
      value: "",
      color: "grey lighten-3",
      events: [],
      isShow: false,
      isToday: dayjs().format("DD"),
      dayText: [],
      text: ["M", "T", "W", "T", "F", "S", "S"],
      user: "莉",
      users: ["莉", "茜"],
    };
  },
  created() {
    this.dayText = this.text.reduce((acc, value, index) => {
      acc.push({
        text: value,
        day: dayjs()
          .day(index + 1)
          .format("DD"),
      });
      return acc;
    }, []);
  },
  methods: {
    getEvents() {
      const events = [];

      events.push({
        name: `${"美甲保養"} \n ${"王小美"}`,
        start: dayjs().add(1, "day").format("YYYY-MM-DD 14:00:00"),
        end: dayjs().add(1, "day").format("YYYY-MM-DD 15:00:00"),
        color: this.color,
        timed: events.end,
      });

      this.events = events;
    },
    changeWeekAndDay(event) {
      this.type = event;
      this.isShow = !this.isShow;
    },
    changeUser(event) {
      this.user = event;
      console.log("user", this.user);
    },
  },
};
</script>
<style lang="scss" scoped>
::v-deep .theme--light.v-btn.v-btn--has-bg {
  width: unset;
  height: unset;
  padding: 10px;
  background-color: #7f74b7;
  // color: white;
  @media (min-width: 786px) {
    padding: 15px;
  }
  @media (min-width: 1024px) {
    padding: 20px;
  }
}

::v-deep .v-event.v-event-start.v-event-end.cyan.white--text {
  width: 5px !important;
  height: 5px !important;
  margin: 5px auto;

  & div {
    display: none;
  }
}

::v-deep .v-event-timed .pl-1 {
  color: #000;
  white-space: pre-line;
}

::v-deep .v-calendar .v-event-timed-container {
  margin: 0;
}

.navbar {
  display: flex;
  align-items: center;
  gap: 8px;
  .calendar_title {
    flex: 2;
    font-size: 20px;
  }
  .change_week_and_day {
    flex: 2;
  }
  .user {
    flex: 1;
  }
}

::v-deep .v-text-field__details {
  display: none;
}

::v-deep .v-input__slot {
  padding: 8px 16px;
  margin: 0;
  border-radius: 20px !important;
}

::v-deep .v-text-field > .v-input__control > .v-input__slot:before {
  border: none;
}

::v-deep .theme--light.v-select .v-select__selections {
  color: #7f74b4;
}

.calendar_header {
  width: calc(100% - 60px);
  height: 60px;
  margin-left: auto;
  padding: 8px 0;
  display: flex;
  .week {
    &_day {
      height: 100%;
      flex: 1 0 0;
      font-size: 12px;
    }
    &_text {
      width: 100%;
      height: 100%;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      &.active {
        background-color: #7f74b4;
        border-radius: 5px;
        color: white;
      }
    }
  }
}
</style>
