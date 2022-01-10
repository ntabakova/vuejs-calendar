<template>
  <div :class="['meetings-item', { main: inCalendarBox }]">
    <div>
      <span class="start">{{ getTime(meeting.start) }}</span>
      <span
        class="end"
        v-if="screenSize > 768 || (screenSize < 768 && !inCalendarBox)"
      >
        - {{ getTime(meeting.end) }}</span
      >
    </div>
    <div
      class="name"
      v-if="screenSize > 1024 || (screenSize < 1024 && !inCalendarBox)"
    >
      {{ meeting.name }} ({{ meeting.meetingRoom }})
    </div>
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
    return {
      screenSize: window.innerWidth,
    };
  },
  created() {
    window.addEventListener("resize", this.onResize);
  },
  methods: {
    getTime(date) {
      return new Date(date).toLocaleTimeString("en-us", {
        timeStyle: "short",
        hour12: false,
        timeZone: "UTC",
      });
    },
    onResize() {
      this.screenSize = window.innerWidth;
    },
  },
};
</script>

<style scoped>
.meetings-item.main {
  font-size: 13px;
}
</style>
