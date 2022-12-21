<template>
  <v-sheet height="1200">
    <v-calendar
      ref="calendar"
      v-model="value"
      :weekdays="weekday"
      :type="type"
      :events="events"
      :event-overlap-mode="mode"
      :locale="EN-US"
      :event-overlap-threshold="60"
      :interval-minutes="60"
      :first-interval="9"
      :interval-count="13"
      :interval-height="100"
      @change="getEvents"
    ></v-calendar>
</v-sheet>
</template>

<script>
  export default {
    name: "Calendar",
    props: {
      type: String,
      mode: String,
      originalEvents: Array,
      events: Array,
      colors: Array,
    },

    data: () => ({
      // events: [],
      weekday: [0, 1, 2, 3, 4, 5, 6],
      value: "",
    }),
    methods: {
      convertIntegerToSeconds(minutes) {
        return minutes * 60;
      },
      convertStringToSeconds(timeString) {
        // Split it at the colons
        let a = timeString.split(':'); 
        // Minutes are worth 60 seconds. Hours are worth 60 minutes.
        let seconds = ((+a[0]) * 60 * 60 + (+a[1]) * 60) * 1000;
        return seconds;
      },
      convertSecondstoDate(minTime, startingTime, durationTime) {
        let time;
        if (durationTime > 0) {
          let durationTimeSeconds = durationTime * 60 * 1000;
          time = minTime.getTime() + this.convertStringToSeconds(startingTime) + durationTimeSeconds;
        } else {
          time = minTime.getTime() + this.convertStringToSeconds(startingTime);
        }
        return new Date(time);
      },
      getEvents ({ start }) {
        const events = [];

        // Set starting time to today's date at midnight
        const min = new Date(`${start.date}T00:00:00`);

        this.originalEvents.forEach( 
          event => events.push({
            name: event.id,
            start: this.convertSecondstoDate(min, event.start),
            end:  this.convertSecondstoDate(min, event.start, event.duration),
            color: this.colors[this.rnd(0, this.colors.length - 1)],
            border: "black, solid, 60px",
            timed: true,
          }),
        );
        this.events = events;
      },
      rnd (a, b) {
        return Math.floor((b - a + 1) * Math.random()) + a
      },
    },
  }
</script>
<style>
.theme--light.v-calendar-events .v-event-timed {
  border: solid black 1px !important;
}
</style>