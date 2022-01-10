<template>
  <div class="meetings-item">
    <span>{{ getTime(meeting.start) }}</span>
    <span :class="['name', { main: inCalendarBox }]"
      >{{ meeting.name }} ({{ meeting.meetingRoom }})</span
    >
  </div>
</template>

<script>
export default {
  name: "Meeting",
  props: {
    meeting: {
      type: Object,
    },
    inCalendarBox: {
      type: Boolean,
    },
  },
  data() {
    return {};
  },
  methods: {
    getTime(date) {
      return new Date(date).toLocaleTimeString("en-us", {
        timeStyle: "short",
        hour12: false,
        timeZone: "UTC",
      });
    },
  },
};
</script>

<style scoped>
.name {
  margin-left: 5px;
}
.name.main {
  display: none;
}
@media (min-width: 1024px) {
  .name.main {
    display: inline;
  }
}
</style>
