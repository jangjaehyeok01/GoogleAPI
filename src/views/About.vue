<template>
  <div>
    <div class="long-title">
      <h3>Tasks for Employees (USA Office)</h3>
    </div>
    <DxScheduler
            :data-source="dataSource"
            :current-date="currentDate"
            :views="views"
            :height="500"
            :editing="false"
            :show-all-day-panel="false"
            :start-day-hour="7"
            start-date-expr="start.dateTime"
            end-date-expr="end.dateTime"
            text-expr="summary"
            time-zone="America/Los_Angeles"
            current-view="month"
    />
  </div>
</template>
<script lang="ts">
  import { Component, Vue } from 'vue-property-decorator'
  import { DxScheduler } from 'devextreme-vue/scheduler'
  import CustomStore from 'devextreme/data/custom_store'
  @Component({
    components: {
      DxScheduler,
    },
  })
  export default class App extends Vue {
    public views = ['day', 'workWeek', 'month']
    public currentDate = new Date()
    public dataSource = new CustomStore({
      load: (options) => this.getData(options, { showDeleted: false }),
    })
    public getData(_: object, requestOptions: object) {
      const PUBLIC_KEY = 'AIzaSyDbO3hnJNAh3CAZXjJqn9sjJpgRwdaae3o'
      const CALENDAR_ID = 'jhjang@uvc.co.kr'
      const dataUrl = [ 'https://www.googleapis.com/calendar/v3/calendars/',
        CALENDAR_ID,
        '/events?key=',
        PUBLIC_KEY,
      ].join('')
      return fetch(dataUrl, requestOptions)
      .then((response) => response.json())
      .then((data) => data.items)
    }
  }
</script>
