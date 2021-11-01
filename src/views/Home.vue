<template>
  <div class="Calendar">
    <div class="month_calendar"></div>
    <div class="weekly_calendar">
      <!-- <pre>{{ events }}</pre> -->
      <v-sheet tile height="54" class="navbar">
        <h1 class="calendar_title" v-if="type === 'week'">預約行事曆</h1>
        <div class="complete_date" v-if="type === 'day'">
          <v-btn icon class="day_button" color="grey darken-2" @click="prev()">
            <i class="fas fa-chevron-left"></i>
          </v-btn>
          {{ completeDate }}
          <v-btn icon class="day_button" @click="next()">
            <i class="fas fa-chevron-right"></i>
          </v-btn>
        </div>
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
      <div class="add_event">
        <v-btn class="add_event_button" @click="addEvent()">新增預約</v-btn>
      </div>
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
      completeDate: dayjs().format("YYYY / MM / DD"),
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
  // watch: {
  //   completeDate() {
  //     console.log(this.completeDate);
  //     this.completeDate = this.completeDate;
  //   },
  // },
  computed: {
    test01() {
      console.log(this.completeDate);
      return this.completeDate;
    },
  },
  methods: {
    getEvents() {
      // const events = [];
      this.events.push({
        name: `${"美甲保養"} \n ${"王小美"}`,
        start: dayjs().format("YYYY-MM-DD 14:00:00"),
        end: dayjs().format("YYYY-MM-DD 15:00:00"),
        color: this.color,
        timed: this.events.end,
      });

      // this.events = events;
    },
    changeWeekAndDay(event) {
      this.type = event;
      this.isShow = !this.isShow;
    },
    changeUser(event) {
      this.user = event;
      console.log("user", this.user);
    },
    prev() {
      this.$refs.calendar.prev();
      // dayjs() 抓的永遠是今天的日期，所以不論執行幾次 add 都會以今天為基準點去加減
      this.completeDate = dayjs(this.completeDate)
        .add(-1, "day")
        .format("YYYY / MM / DD");
    },
    next() {
      this.$refs.calendar.next();
      this.completeDate = dayjs(this.completeDate)
        .add(1, "day")
        .format("YYYY / MM / DD");
    },
    addEvent() {
      this.events.push({
        name: `${"美甲保養"} \n ${"王小美"}`,
        start: dayjs().add(1, "day").format("YYYY-MM-DD 14:00:00"),
        end: dayjs().add(1, "day").format("YYYY-MM-DD 15:00:00"),
        color: this.color,
        timed: this.events.end,
      });
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
  padding: 0 8px;
  display: flex;
  align-items: center;
  gap: 8px;
  @media (min-width: 768px) {
    padding: 40px;
  }
  .calendar_title {
    flex: 2;
    font-size: 20px;
  }
  .complete_date {
    flex: 2;
    font-size: 14px;
    @media (min-width: 768px) {
      font-size: 20px;
    }
    .day_button {
      width: 16px;
      @media (min-width: 768px) {
        width: 32px;
      }
    }
  }
  .change_week_and_day {
    flex: 2;
    @media (min-width: 768px) {
      flex: 0 0 20%;
    }
  }
  .user {
    flex: 1;
    @media (min-width: 768px) {
      flex: 0 0 10%;
    }
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

.add_event {
  padding: 16px 0;
  display: flex;
  justify-content: center;
}

::v-deep .theme--light.v-btn.v-btn--has-bg {
  padding: 20px 40px;
  color: white;
}
</style>
