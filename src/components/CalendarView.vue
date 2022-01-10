<template>
  <div class="container">
    <h1 class="heading">Calendar</h1>
    <div class="month-selector">
      <button @click="showPrevMonth">&lt;</button>
      <span>{{ currentFullMonth }} {{ currentYear }}</span>
      <button @click="showNextMonth">&gt;</button>
    </div>

    <div class="calendar">
      <div v-for="day in weekDays" :key="day" class="calendar-weekdays">
        {{ day }}
      </div>
      <CalendarBox
        v-for="date in totalBoxes"
        :key="date"
        :date="date"
        :calendarData="calendarData"
        :currentCalendar="currentCalendar"
      />
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
      let weekDay = firstDayOfMonth.toLocaleDateString("en-us", {
        weekday: "short",
      });

      // empty boxes of the week before start of the month
      this.daysOfPrevMonth = this.weekDays.indexOf(weekDay);

      for (let i = 1; i <= this.daysOfPrevMonth; i++) {
        this.totalBoxes.push({ type: "prev" });
      }

      for (let i = 1; i <= this.daysOfCurrentMonth; i++) {
        this.totalBoxes.push({
          type: "current",
          day: i,
          date: new Date(this.currentYear, this.currentMonth, i),
        });
      }
    },
    showPrevMonth() {
      this.currentCalendar = new Date(this.currentYear, this.currentMonth - 1);
    },
    showNextMonth() {
      this.currentCalendar = new Date(this.currentYear, this.currentMonth + 1);
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
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  text-align: center;
}
.calendar-weekdays {
  padding: 5px;
}
.calendar-box {
  padding: 8px 5px;
  min-height: 112px;
  border: 1px solid #ede5e56b;
}
.meetings-item {
  margin: 10px 0;
  color: #919191;
}
@media (min-width: 1024px) {
  .container {
    margin: 0 15px;
  }
  .calendar-box {
    min-height: 160px;
  }
}
.month-selector {
  display: flex;
  justify-content: center;
  align-items: center;
  max-width: 275px;
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
