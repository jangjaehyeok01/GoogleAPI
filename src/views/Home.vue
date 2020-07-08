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
<script>
  import 'whatwg-fetch'
  import DxScheduler from 'devextreme-vue/scheduler'
  import CustomStore from 'devextreme/data/custom_store'

  export default {
    components: {
      DxScheduler
    },
    data() {
      return {
        views: ['day', 'workWeek', 'month'],
        currentDate: new Date(),
        dataSource: new CustomStore({
          load: (options) => this.getData({_: options, requestOptions: {showDeleted: false}})
        })
      }
    },
    methods: {
      getData(_, requestOptions) {
        const PUBLIC_KEY = 'AIzaSyDbO3hnJNAh3CAZXjJqn9sjJpgRwdaae3o',
                CALENDAR_ID = 'jhjang@uvc.co.kr'
        const dataUrl = [  'https://calendar.google.com/calendar/',
          CALENDAR_ID, '/events?key=', PUBLIC_KEY].join('')

        return fetch(dataUrl, requestOptions).then(
                (response) => response.json()
        ).then((data) => data.items)
      }
    }
  }
</script>
<style>
  .long-title h3 {
    font-weight: 200;
    font-size: 28px;
    text-align: center;
    margin-bottom: 20px;
  }
</style>
