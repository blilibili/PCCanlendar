<template>
  <div id="app">
    <div>日历: 实例</div>
    <div>
        <Calendar
                :work-memoir-list="MockData"
                :select-month-obj="selectMonthObj"
        />
    </div>
  </div>
</template>

<script>
import Calendar from './components/Calendar/entry'
import {workList} from './components/Calendar/mockData'

export default {
  name: 'App',
  components: {
    Calendar
  },
  data() {
    return {
      MockData: workList,
      selectMonthObj: {
        year: new Date().getFullYear(),
        month: `${new Date().getMonth() + 1 > 10 ? (new Date().getMonth() + 1) : '0' + (new Date().getMonth() + 1)}`
      }
    }
  },
  mounted() {
    console.log('MockData', this.MockData)
    this.selectMonthObj = {
      year: new Date().getFullYear(),
      month: new Date().getMonth() + 1
    }
    this.selectMonthObj.month = `${this.selectMonthObj.month > 10 ? this.selectMonthObj.month : '0' + this.selectMonthObj.month}`

  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
