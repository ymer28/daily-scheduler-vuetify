<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="center">
        <h1> My today's agenda </h1>
      </div>
    </v-app-bar>

    <v-main>
      <v-sheet height="600">
        <v-calendar
          ref="calendar"
          v-model="value"
          :weekdays="weekday"
          :type="type"
          :events="events"
          :event-overlap-mode="mode"
          :event-overlap-threshold="30"
          :event-color="getEventColor"
          @change="getEvents"
        ></v-calendar>
      </v-sheet>
    </v-main>
  </v-app>
</template>

<script>
//import HelloWorld from './components/HelloWorld';

export default {
  name: 'App',

  components: {
    //HelloWorld,
  },
    data: () => ({
      type: 'day',
      //types: ['month', 'week', 'day', '4day'],
      mode: 'column',
      //modes: ['stack', 'column'],
      weekday: [0, 1, 2, 3, 4, 5, 6],
      weekdays: [
        { text: 'Sun - Sat', value: [0, 1, 2, 3, 4, 5, 6] },
        { text: 'Mon - Sun', value: [1, 2, 3, 4, 5, 6, 0] },
        { text: 'Mon - Fri', value: [1, 2, 3, 4, 5] },
        { text: 'Mon, Wed, Fri', value: [1, 3, 5] },
      ],
      value: '',
      events: [],
        originalEvents: [
        {
          "id": 1,
          "start": "17:00",
          "duration": 60
        },
        {
          "id": 2,
          "start": "17:00",
          "duration": 120
        },
        {
          "id": 3,
          "start": "19:40",
          "duration": 10
        },
        {
          "id": 4,
          "start": "15:00",
          "duration": 20
        },
        {
          "id": 5,
          "start": "18:00",
          "duration": 60
        },
        {
          "id": 6,
          "start": "10:25",
          "duration": 35
        },
        {
          "id": 7,
          "start": "10:45",
          "duration": 30
        },
        {
          "id": 8,
          "start": "17:00",
          "duration": 60
        },
        {
          "id": 9,
          "start": "10:00",
          "duration": 30
        },
        {
          "id": 10,
          "start": "11:50",
          "duration": 20
        },
        {
          "id": 11,
          "start": "19:00",
          "duration": 60
        },
        {
          "id": 12,
          "start": "09:00",
          "duration": 45
        },
        {
          "id": 13,
          "start": "14:45",
          "duration": 60
        },
        {
          "id": 14,
          "start": "19:20",
          "duration": 10
        },
        {
          "id": 15,
          "start": "11:50",
          "duration": 30
        },
        {
          "id": 16,
          "start": "11:40",
          "duration": 40
        },
        {
          "id": 17,
          "start": "14:00",
          "duration": 30
        }
      ],
      colors: ['blue', 'indigo', 'deep-purple', 'cyan', 'green', 'orange', 'grey darken-1'],
      names: ['Meeting', 'Holiday', 'PTO', 'Travel', 'Event', 'Birthday', 'Conference', 'Party'],
    }),
    methods: {
      // getEvents ({ start, end }) {
      //   console.log("START-VALUE", start);
      //   console.log("END-VALUE", end);
      //   const events = [];

      //   // Set starting time to today's date at midnight
      //   const min = new Date(`${start.date}T00:00:00`);
      //   // Set finishing time to today's date at 23:59:59
      //   const max = new Date(`${end.date}T23:59:59`);
      //   console.log("max.getTime()", max.getTime());
      //   console.log("min.getTime()", min.getTime());
      //   const days = (max.getTime() - min.getTime()) / 86400000;
      //   console.log("DAYS-value", days);
      //   // Generate a random number of events
      //   const eventCount = this.rnd(days, days + 20)
      //   console.log("eventCount", eventCount)

      //   for (let i = 0; i < eventCount; i++) {
      //     const allDay = this.rnd(0, 3) === 0
      //     console.log("allDay", allDay);
      //     console.log("!allDay", !allDay);
      //     const firstTimestamp = this.rnd(min.getTime(), max.getTime())
      //     console.log("firstTimestamp", firstTimestamp);
      //     const first = new Date(firstTimestamp - (firstTimestamp % 900000))
      //     console.log("FIRST", first);
      //     const secondTimestamp = this.rnd(2, allDay ? 288 : 8) * 900000
      //     const second = new Date(first.getTime() + secondTimestamp)

      //     events.push({
      //       name: this.names[this.rnd(0, this.names.length - 1)],
      //       start: first,
      //       end: second,
      //       color: this.colors[this.rnd(0, this.colors.length - 1)],
      //       timed: !allDay,
      //     })
      //   }

      //   this.events = events
      //   console.log("this.events",this.events);
      // },
      convertIntegerToSeconds(minutes) {
        return minutes * 60;
      },
      convertStringToSeconds(timeString) {
        // Split it at the colons
        let a = timeString.split(':'); 
        // Minutes are worth 60 seconds. Hours are worth 60 minutes.
        let seconds = ((+a[0]) * 60 * 60 + (+a[1]) * 60) * 1000;
        console.log("FUCKING SECONDS", seconds);
        return seconds;
      },
      convertSecondstoDate(minTime, startingTime, durationTime) {
        let time;
        if (durationTime > 0) {
          let DurationTimeSeconds = durationTime * 60 * 1000;
          time = minTime.getTime() + this.convertStringToSeconds(startingTime) + DurationTimeSeconds;
          console.log("TIME@@@@@@@@", time);
          console.log("new Date(time)@@@@@", new Date(time))
        } else {
          time = minTime.getTime() + this.convertStringToSeconds(startingTime);
          console.log("TIME", time);
          console.log("new Date(time)", new Date(time))
        }
        return new Date(time);
      },
      getEvents ({ start, end }) {
        console.log("START-VALUE", start);
        console.log("END-VALUE", end);
        const events = [];

        // Set starting time to today's date at midnight
        const min = new Date(`${start.date}T00:00:00`);
        // Set finishing time to today's date at 23:59:59
        const max = new Date(`${end.date}T23:59:59`);
        console.log("max.getTime()", max.getTime());
        console.log("min.getTime()", min.getTime());
        const days = (max.getTime() - min.getTime()) / 86400000;
        console.log("DAYS-value", days);

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
        console.log("this.events",this.events);
      },
      getEventColor (event) {
        return event.color
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
</style>
