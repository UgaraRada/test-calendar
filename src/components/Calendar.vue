<template>
  <v-container class="d-flex">
    <v-card class="mr-7">
    <div id="inspire">
      <div>
        <v-sheet
          tile
          height="54"
          class="d-flex calendar"
        >
          <v-toolbar-title v-if="$refs.calendar" class="ma-2 ml-4">
              {{ $refs.calendar.title }}
          </v-toolbar-title>
          <v-spacer></v-spacer>
          <v-btn
            icon
            class="ma-2"
            @click="$refs.calendar.prev()"
          >
            <v-icon>mdi-chevron-left</v-icon>
          </v-btn>

          <v-btn
            icon
            class="ma-2"
            @click="$refs.calendar.next()"
          >
            <v-icon>mdi-chevron-right</v-icon>
          </v-btn>
        </v-sheet>
        <v-sheet height="600">
          <v-calendar
            ref="calendar"
            v-model="value"
            :weekdays="weekday"
            :type="type"
            :events="events"
            :event-overlap-mode="mode"
            :event-overlap-threshold="30"
            @change="getEvents"
            locale="ru"
            :now="today"
          ></v-calendar>
        </v-sheet>
      </div>
    </div>
    </v-card>
    <v-card class="align-self-start">
      <v-card-title>Сотрудник</v-card-title>
      <v-select
          :items="usersName"
          outlined
          v-model="activeUser"
          @change="getEvents"
        ></v-select>
    </v-card>
  </v-container>
</template>

<script>
import uniqid from 'uniqid';

export default {
  name: 'Calendar',

  data: () => ({
    today: new Date().toISOString().substr(0, 10),
    type: 'month',
    mode: 'stack',
    weekday: [1, 2, 3, 4, 5, 6, 0],
    value: '',
    dayFormat: 'ru',
    events: [],
    users: [],
    weekends: [],
    activeUser: '',
    colors: ['blue', 'indigo', 'deep-purple', 'cyan', 'green', 'orange', 'brown darken-1'],

  }),
  computed: {
    usersName() {
      return this.users.map((it) => it.name);
    },
    selectUser() {
      return this.users.filter((it) => it.name === this.activeUser)[0];
    },
  },
  methods: {
    getEvents() {
      this.getWeekends();
      if (this.activeUser) {
        const eventsUser = [];
        this.selectUser.trips.forEach((it) => {
          const appDataUser = {
            name: it.city,
            start: it.data,
            end: it.data,
            color: this.colors[Math.floor(it.city.length * 0.5)],
          };
          eventsUser.push(appDataUser);
        });
        this.events = this.events.concat(eventsUser);
      }
    },
    getWeekends() {
      const eventWeekends = [];
      this.weekends.forEach((it) => {
        const appDataWeekends = {
          name: it.title,
          start: it.data,
          end: it.data,
          color: 'grey',
        };
        eventWeekends.push(appDataWeekends);
      });
      this.events = eventWeekends;
    },
    getDates() {
      this.users = [{
        name: 'Алексей Лечащий',
        id: uniqid(),
        trips: [
          {
            data: '2021-12-01',
            city: 'Екатеринбург',
          },
          {
            data: '2021-12-02',
            city: 'Екатеринбург',
          },
          {
            data: '2021-12-06',
            city: 'Москва',
          },
        ],
      },
      {
        name: 'Александр Выздоравливающий',
        id: uniqid(),
        trips: [
          {
            data: '2021-12-10',
            city: 'Тула',
          },
          {
            data: '2021-12-02',
            city: 'Москва',
          },
          {
            data: '2021-12-06',
            city: 'Астрахань',
          },
        ],
      }];
      this.weekends = [
        {
          data: '2021-12-04',
          title: 'Выходной',
        },
        {
          data: '2021-12-05',
          title: 'Выходной',
        },
        {
          data: '2021-12-11',
          title: 'Выходной',
        },
        {
          data: '2021-12-12',
          title: 'Выходной',
        },
        {
          data: '2021-12-18',
          title: 'Выходной',
        },
        {
          data: '2021-12-19',
          title: 'Выходной',
        },
        {
          data: '2021-12-25',
          title: 'Выходной',
        },
        {
          data: '2021-12-26',
          title: 'Выходной',
        },
        {
          data: '2022-01-01',
          title: 'Праздники',
        },
        {
          data: '2022-01-02',
          title: 'Праздники',
        },
      ];
    },
  },
  mounted() {
    this.getDates();
    this.getEvents();
    this.getWeekends();
  },

};
</script>

<style scoped>
  .calendar {
    width: 900px;
  }

</style>
