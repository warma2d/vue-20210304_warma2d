<template>
  <calendar-view
    v-slot="slotProps"
  >
    <router-link
      v-for="meetup in dayMeetups(slotProps.dayDate)"
      :key="meetup.date + getRandomArbitrary(1, 99999)"
      :to="{ name: 'meetup', params: { meetupId: meetup.id } }"
      class="rangepicker__event"
    >{{ meetup.title }}</router-link>
  </calendar-view>
</template>

<script>
import CalendarView from './CalendarView';

export default {
  name: 'MeetupsCalendar',

  components: {
    CalendarView,
  },

  props: {
    meetups: {
      type: Array,
      required: true,
    },
  },

  methods: {
    getRandomArbitrary(min, max) {
      return Math.random() * (max - min) + min;
    },

    isEqualDay(date1, date2) {
      return date1.getDate() === date2.getDate()
        && date1.getMonth() === date2.getMonth()
        && date1.getFullYear() === date2.getFullYear()
    },

    dayMeetups(date) {
      let dayMeetups = []
      for (let meetup of this.meetups) {
        if (this.isEqualDay(date, new Date(meetup.date))) {
          dayMeetups.push(meetup)
        }
      }

      return dayMeetups
    },
  },
};
</script>

<style scoped>
.rangepicker__event {
  --maxLines: 2;
  --lineHeight: 16px;
  --fontSize: 14px;

  display: block;

  text-align: left;
  text-overflow: ellipsis;
  overflow: hidden;
  font-size: var(--fontSize);
  line-height: var(--lineHeight);
  font-weight: 600;
  color: var(--white);
  padding: 4px 6px;
  border-radius: 2px;
  background-color: var(--blue);
  text-decoration: none;
  margin-top: 4px;
}

@media all and (min-width: 767px) {
  .rangepicker__event {
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    max-height: calc(var(--maxLines) * var(--lineHeight) + 6px);
  }
}
</style>
