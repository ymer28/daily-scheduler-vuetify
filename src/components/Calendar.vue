<template>
  <v-sheet height="1200">
    <v-calendar
      ref="calendar"
      v-model="value"
      locale="en"
      first-interval="9"
      interval-count="12"
      :weekdays="weekday"
      :type="type"
      :events="events"
      :event-overlap-mode="mode"
      :event-overlap-threshold="60"
      :interval-minutes="60"
      :interval-height="100"
      @change="getEvents"
    >
      <template class="event" v-slot:event="{ event }">
        <div class="event__name">
          {{event.name}}
        </div>
      </template>
    </v-calendar>
</v-sheet>
</template>

<script>
  export default {
    name: "Calendar",
    props: {
      type: String,
      mode: String,
      originalEvents: Array,
      colors: Array,
    },
    data: () => ({
      weekday: [0, 1, 2, 3, 4, 5, 6],
      value: "",
      events: [],
    }),
    methods: {
      convertTimeToSeconds(time, isMinute) {
        if (isMinute) {
          return time * 60 * 1000;
        }
          return time * 60 * 60 * 1000;

      },
      convertStringToSeconds(timeString) {
        // Split it at the colons
        let a = timeString.split(':'); 
        // Minutes are worth 60 seconds. Hours are worth 60 minutes.
        let seconds = this.convertTimeToSeconds(a[0])+ this.convertTimeToSeconds(a[1]);
        return seconds;
      },
      convertSecondstoDate(minTime, startingTime, durationTime) {
        let time;
        if (durationTime > 0) {
          let durationTimeSeconds = this.convertTimeToSeconds(durationTime, true);
          time = minTime.getTime() + this.convertStringToSeconds(startingTime) + durationTimeSeconds;
        } else {
          time = minTime.getTime() + this.convertStringToSeconds(startingTime);
        }
        return new Date(time);
      },
      getEvents({ start }) {
        const events = [];

        // Set starting time to today's date at midnight
        const min = new Date(`${start.date}T00:00:00`);

        this.originalEvents.forEach( 
          event => events.push({
            name: event.id,
            start: this.convertSecondstoDate(min, event.start),
            end:  this.convertSecondstoDate(min, event.start, event.duration),
            color: this.colors[this.rnd(0, this.colors.length - 1)],
            timed: true,
          }),
        );
        this.events = events;
      },
      rnd(a, b) {
        return Math.floor((b - a + 1) * Math.random()) + a
      },
    },
  }
</script>
<style>
  div.theme--light.v-calendar-events .v-event-timed {
    border: solid white 2px !important;
    box-shadow:
      inset 0 0 0 1px rgb(10, 10, 10);
    width: 100%;
  }
  .v-calendar .v-event-timed-container {
    margin-right: 0;
  }
  .v-event-timed {
    display: flex;
    justify-content: center;
    align-items: center;
  }
  div.event__name {
    display: flex;
    justify-content: center;
    align-items: center;
  }
</style>