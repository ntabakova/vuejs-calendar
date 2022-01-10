<template>
  <div :class="['calendar-box', { 'has-extra': allMeetings.length > 3 }]">
    <div :class="['calendar-box-inner']" @click="showModal">
      <div :class="['day', date.type]">{{ date.day }}</div>
      <div class="meetings" v-if="allMeetings[0]">
        <Meeting
          v-for="meeting in firstMeetings"
          :key="JSON.stringify(meeting)"
          :meeting="meeting"
          :in-calendar-box="true"
        />
      </div>
    </div>

    <MeetingsModal
      v-if="modalIsOpen"
      @close="modalIsOpen = false"
      :meetings="allMeetings"
      :date="date"
    />
  </div>
</template>

<script>
import Meeting from "./Meeting.vue";
import MeetingsModal from "./MeetingsModal.vue";

export default {
  name: "CalendarBox",
  props: {
    date: {
      type: Object,
    },
    calendarData: {
      type: Object,
    },
    currentCalendar: {
      type: Date,
    },
  },
  components: {
    Meeting,
    MeetingsModal,
  },
  data() {
    return {
      allMeetings: [],
      firstMeetings: [],
      modalIsOpen: false,
    };
  },
  created() {
    this.filterMeetings(this.date);
  },
  methods: {
    filterMeetings(date) {
      this.allMeetings = this.calendarData.meetings
        .filter(
          (x) =>
            new Date(x.start) >= date.date &&
            new Date(x.start) <
              new Date(
                this.currentCalendar.getFullYear(),
                this.currentCalendar.getMonth(),
                date.day + 1
              )
        )
        .sort((x, y) => {
          return new Date(x.start) - new Date(y.start);
        });

      // show only the first 3 meetings
      this.firstMeetings = this.allMeetings.filter((x, index) => index < 3);
    },
    showModal() {
      if (this.allMeetings.length > 3) this.modalIsOpen = true;
    },
  },
};
</script>
