<template>
  <div class="container">
    <h1 class="heading">Calendar</h1>
    <div class="month-selector">
      <button @click="showPrevMonth">&lt;</button>
      <span>{{ currentFullMonth }} {{ currentYear }}</span>
      <button @click="showNextMonth">&gt;</button>
    </div>

    <div class="calendar">
      <div class="row">
        <div v-for="day in weekDays" :key="day" class="calendar-weekdays">
          {{ day }}
        </div>
      </div>

      <div class="calendar-inner">
        <div
          class="row"
          v-for="row in Math.round(totalBoxes.length / 7)"
          :key="row"
        >
          <CalendarBox
            v-for="date in filterBoxes(row, 7)"
            :key="date"
            :date="date"
            :calendarData="calendarData"
            :currentCalendar="currentCalendar"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import calendarData from "../assets/data.json";
import CalendarBox from "./CalendarBox.vue";

export default {
  name: "CalendarView",
  components: {
    CalendarBox,
  },
  props: {},
  data() {
    return {
      calendarData: calendarData,
      weekDays: ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"],
      currentCalendar: new Date(2020, 5, 1),
      daysOfPrevMonth: null,
      daysOfCurrentMonth: null,
      daysOfNextMonth: null,
      totalBoxes: [],
    };
  },
  created() {
    this.handleData();
  },
  watch: {
    currentMonth() {
      this.handleData();
    },
  },
  computed: {
    currentFullMonth() {
      return this.currentCalendar.toLocaleDateString("en-us", {
        month: "long",
      });
    },
    currentMonth() {
      return this.currentCalendar.getMonth();
    },
    currentYear() {
      return this.currentCalendar.getFullYear();
    },
  },
  methods: {
    handleData() {
      this.totalBoxes = [];
      this.daysOfCurrentMonth = new Date(
        this.currentYear,
        this.currentMonth + 1,
        0
      ).getDate();

      let firstDayOfMonth = new Date(this.currentYear, this.currentMonth, 1);
      let firstWeekDay = firstDayOfMonth.toLocaleDateString("en-us", {
        weekday: "short",
      });

      let lastDayOfMonth = new Date(this.currentYear, this.currentMonth + 1, 0);
      let lastWeekDay = lastDayOfMonth.toLocaleDateString("en-us", {
        weekday: "short",
      });

      // empty days of the week before start and after end of the month
      this.daysOfPrevMonth = this.weekDays.indexOf(firstWeekDay);
      this.daysOfNextMonth =
        this.weekDays.length - this.weekDays.indexOf(lastWeekDay) - 1;

      let lastDayOfPrevMonth = new Date(
        this.currentYear,
        this.currentMonth,
        0
      ).getDate();

      for (let i = 1; i <= this.daysOfPrevMonth; i++) {
        this.totalBoxes.push({ type: "prev", day: lastDayOfPrevMonth });
        lastDayOfPrevMonth--;
      }

      this.totalBoxes.reverse();

      for (let i = 1; i <= this.daysOfCurrentMonth; i++) {
        this.totalBoxes.push({
          type: "current",
          day: i,
          date: new Date(this.currentYear, this.currentMonth, i),
        });
      }

      for (let i = 1; i <= this.daysOfNextMonth; i++) {
        this.totalBoxes.push({ type: "next", day: i });
      }
    },
    showPrevMonth() {
      this.currentCalendar = new Date(this.currentYear, this.currentMonth - 1);
    },
    showNextMonth() {
      this.currentCalendar = new Date(this.currentYear, this.currentMonth + 1);
    },
    filterBoxes(row, weekDays) {
      return this.totalBoxes.filter(
        (x, i) => i >= weekDays * (row - 1) && i < weekDays * row
      );
    },
  },
};
</script>

<style>
html {
  font-family: sans-serif;
}
.heading {
  text-align: center;
}
.calendar {
  text-align: center;
}

.calendar-inner {
  border: 1px solid #e0dcdc;
}

.row {
  display: flex;
}
.calendar-weekdays {
  padding: 5px;
  flex: 1;
}

.calendar-box {
  padding: 8px 5px;
  flex: 1;
  border-bottom: 1px solid #e0dcdc;
  border-right: 1px solid #e0dcdc;
  min-height: 110px;
}

.row .calendar-box:last-of-type {
  border-right: none;
}

.calendar-inner .row:last-of-type .calendar-box {
  border-bottom: none;
}

.meetings-item {
  margin: 10px 0;
  color: #919191;
}
@media (min-width: 1024px) {
  .container {
    margin: 0 15px;
  }
}
.month-selector {
  display: flex;
  justify-content: center;
  align-items: center;
  max-width: 280px;
  margin-bottom: 20px;
  font-size: 20px;
  text-align: center;
}

.month-selector button {
  flex: 1;
  cursor: pointer;
  background: none;
  border: none;
  font-size: 18px;
}

.month-selector span {
  flex: 3;
}
</style>
