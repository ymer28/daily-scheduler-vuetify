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
        :original-events="originalEvents" 
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
  components: { Calendar },
  data: () => ({
    pageTitle: "My today's agenda",
    type: "day",
    events: [],
    originalEvents: originalEvents,
    mode: "column",
    colors: ['blue', 'indigo', 'deep-purple', 'cyan', 'green', 'orange', 'grey darken-1'],
  }),
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
