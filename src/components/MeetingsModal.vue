<template>
  <div class="modal-overlay" @click="closeModal">
    <div class="modal-content" @click.stop="">
      <div class="date">
        {{ formatDate(date) }}
      </div>
      <span class="close" @click="closeModal">&times;</span>
      <div class="meetings">
        <Meeting
          v-for="meeting in meetings"
          :key="JSON.stringify(meeting)"
          :meeting="meeting"
        />
      </div>
    </div>
  </div>
</template>

<script>
import Meeting from "./Meeting.vue";

export default {
  name: "MeetingsModal",
  props: {
    meetings: {
      type: Array,
    },
    date: {
      type: Object,
    },
  },
  components: {
    Meeting,
  },
  data() {
    return {};
  },

  methods: {
    closeModal() {
      this.$emit("close");
    },
    formatDate(date) {
      let weekDay = date.date.toLocaleDateString("en-us", {
        weekday: "long",
      });

      let month = date.date.toLocaleDateString("en-us", {
        month: "long",
      });
      let year = date.date.getFullYear();
      let day = date.day;

      return `${weekDay}, ${month} ${day} ${year}`;
    },
  },
};
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  z-index: 100;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
}

.modal-content {
  background-color: #fefefe;
  margin: auto;
  padding: 20px;
  width: 80%;
  max-width: 500px;
  position: relative;
  top: 21%;
  box-shadow: 0px 5px 25px 5px #afabab7a;
}
.date {
  margin-bottom: 20px;
  font-size: 20px;
}
.close {
  position: absolute;
  right: 9px;
  top: 1px;
  font-size: 30px;
  color: #919191;
  cursor: pointer;
}
</style>
