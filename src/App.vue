<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="center">
        <span class="page__title"> {{ pageTitle }} </span>
      </div>
    </v-app-bar>

    <v-main>
      <calendar 
        :type="type" 
        :mode="mode" 
        :events="events" 
        :originalEvents="originalEvents" 
        :colors="colors"
      />
    </v-main>
  </v-app>
</template>

<script>
import Calendar from './components/Calendar.vue';
import { originalEvents } from './originalEvents.js'

export default {
  name: 'App',

  components: {
    Calendar,
  },
    data: () => ({
      pageTitle: "My today's agenda",
      type: "day",
      events: [],
      originalEvents: originalEvents,
      mode: "column",
      colors: ['blue', 'indigo', 'deep-purple', 'cyan', 'green', 'orange', 'grey darken-1'],
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
  .center {
    text-align: center;
    width: 100%;
  }
  .page__title {
    font-size: 1.2rem;
  }
  @media (min-width: 20rem) {
    .page__title {
      font-size: 2rem;
    }
  }
  @media (min-width: 40rem) {
    .page__title {
      font-size: 3rem;
    }
  }
</style>
