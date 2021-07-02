<template>
  <div class="calendar-month-container">
    <div class="calendar-month-headers-row">
      <div class="calendar-month-headers">日</div>
      <div class="calendar-month-headers">一</div>
      <div class="calendar-month-headers">二</div>
      <div class="calendar-month-headers">三</div>
      <div class="calendar-month-headers">四</div>
      <div class="calendar-month-headers">五</div>
      <div class="calendar-month-headers">六</div>
    </div>

    <div v-for="(item, index) in currentMonthArr" :key="index" class="calendar-month-headers-row">
      <div v-for="(child, key) in item" :key="key" class="calendar-month-body">
        <div class="mask-add-count">
          <template>
            +{{ child.memoirTaskList.length }}
          </template>
        </div>
        <div class="calendar-month-date" :style="currentMonth !== child.month?'color: #5D626D':''">{{ child.day }}</div>
        <template v-if="child.memoirTaskList && child.memoirTaskList.length > 0">
          <template v-for="(im, ix) in child.memoirTaskList">
            <div
              v-if="im.workContent !== ''"
              :key="key+ix"
              class="task-label"
              style="color: #092D4B;background-color: #C4E2FB;margin-top: 6px;"
              :class="['A', 'B'].indexOf(im.selfLevel) === -1? 'blue-bg': 'green-bg'"
              @click="showDetailInfo(im)"
            >
              {{ im.workContent }}
            </div>
          </template>
        </template>

        <template v-else-if="child.isWorkDay === 0">
          <div class="weekend-words">
            休
          </div>
          <div class="weekend-mask">

          </div>
        </template>
      </div>
    </div>
  </div>
</template>

<script>
import CalendarObj from './date.js'
export default {
  name: 'Entry',
  props: {
    selectMonthObj: {
      type: Object,
      default: () => {
        return {
          year: new Date().getFullYear(),
          month: new Date().getMonth()
        }
      }
    },
    // 实录工作list
    workMemoirList: {
      type: Array,
      default: () => []
    }
  },
  data() {
    return {
      newMonthArr: {},
      currentMonthArr: [],
      weekArr: [],
      currentMonth: '',
      currentDateList: []
    }
  },
  watch: {
    '$props.selectMonthObj': {
      deep: true,
      immediate: true,
      handler() {
        this.initMethods()
      }
    },
    workMemoirList: {
      deep: true,
      immediate: true,
      handler(val) {
        for (let i = 0; i < this.currentMonthArr.length; i++) {
          for (let j = 0; j < this.currentMonthArr[i].length; j++) {
            this.currentMonthArr[i][j].memoirTaskList = []
            if (this.currentMonthArr[i][j].week === 0 || this.currentMonthArr[i][j].week === 6) {
              this.currentMonthArr[i][j].isWorkDay = 0
            }
          }
        }
        for (let i = 0; i < this.currentMonthArr.length; i++) {
          for (let j = 0; j < this.currentMonthArr[i].length; j++) {
            // 匹配数组到日历中去
            val.forEach((item) => {
              if (item.createTime.split(' ')[0] === this.currentMonthArr[i][j].formatDate) {
                const tmpObj = {
                  actuallyTime: item.actuallyTime,
                  selfLevel: item.selfLevel,
                  checkLevel: item.checkLevel, // 考评
                  checkReason: item.checkReason, // 考评原因
                  selfReason: item.selfReason, // 自评原因
                  projectUrl: item.projectUrl, // 项目成果
                  id: item.id,
                  taskId: item.taskId,
                  planId: item.planId,
                  type: item.type,
                  planTimeDetail: item.planTimeDetail,
                  lastReason: item.lastReason,
                  lastLevel: item.lastLevel,
                  planTime: item.planTime,
                  season: item.season,
                  isWorkDay: 1,
                  postTaskVOList: item.postTaskVOList,
                  repulseStatus: item.repulseStatus
                }
                if (item.postTaskVOList.length > 0) {
                  let tmpWords = ''
                  item.postTaskVOList.forEach((child) => {
                    if(child) {
                      tmpWords = `${tmpWords}${child.taskDesc}`
                    }
                  })
                  tmpObj.workContent = tmpWords
                  this.currentMonthArr[i][j].memoirTaskList.push(tmpObj)
                }
              }
            })

            // 赋值对应的
          }
        }
        // 匹配数组到日历中去
        // val.forEach((item) => {
        //   for (let i = 0; i < this.currentMonthArr.length; i++) {
        //     for (let j = 0; j < this.currentMonthArr[i].length; j++) {
        //       if (item.createTime.split(' ')[0] === this.currentMonthArr[i][j].formatDate) {
        //         console.log('匹配到', item)
        //         this.currentMonthArr[i][j].actuallyTime = item.actuallyTime // 实际用时
        //         this.currentMonthArr[i][j].selfLevel = item.selfLevel // 自评
        //         this.currentMonthArr[i][j].checkLevel = item.checkLevel // 考评
        //         this.currentMonthArr[i][j].checkReason = item.checkReason // 考评原因
        //         this.currentMonthArr[i][j].selfReason = item.selfReason // 自评原因
        //         this.currentMonthArr[i][j].projectUrl = item.projectUrl // 项目成果
        //         this.currentMonthArr[i][j].id = item.id
        //         this.currentMonthArr[i][j].taskId = item.taskId
        //         this.currentMonthArr[i][j].planId = item.planId
        //         this.currentMonthArr[i][j].type = item.type
        //
        //         this.currentMonthArr[i][j].planTimeDetail = item.planTimeDetail
        //         this.currentMonthArr[i][j].planTime = item.planTime
        //         this.currentMonthArr[i][j].season = item.season
        //
        //         this.currentMonthArr[i][j].isWorkDay = 1
        //         if (item.postTaskVOList.length > 0) {
        //           let tmpWords = ''
        //           item.postTaskVOList.forEach((child, childKey) => {
        //             tmpWords = `${tmpWords}${child.taskDesc}`
        //           })
        //           this.currentMonthArr[i][j].postTaskVOList.push(tmpWords)
        //         }
        //       }
        //     }
        //   }
        // })
        this.currentMonthArr.splice(0, 0)

        console.log('currentMonthArr --------', this.currentMonthArr)
      }
    }
  },
  mounted() {
    this.currentMonth = new Date().getMonth() + 1
    // this.initMethods()
  },
  methods: {
    showDetailInfo(row) {
      console.log('输入详情 --------', row)
      this.$emit('showDetailInfo', row)
    },
    initMethods() {
      this.currentMonthArr = []
      let weekIndex = 0
      let count = 0
      console.log(count)
      const calendar = CalendarObj({
        // 获取的每个日历数据的最小单元
        // 这里可以自定义想要的数据

        // callback 参数为最终的日历josn 数据
        callback: calendarJson => {
          this.currentDateList = calendarJson
          this.currentDateList.forEach((item) => {
            item.formatDate = `${item.year}-${item.month >= 10 ? item.month : '0' + item.month}-${item.day >= 10 ? item.day : '0' + item.day}`
            count++
            // 初始化percent
            item.weekIndex = weekIndex
            item.postTaskVOList = []
            this.weekArr.push(item)
            if (item.week === 6) {
              weekIndex++
              this.currentMonthArr.push(this.weekArr)
              this.weekArr = []
            }
            item.percent = 0
          })

          // this.currentMonth = calendarJson
        }
        // 设置 dateStr 输出格式
      }).format('YYYY-MM-dd')

      console.log('selectMOnthObj', this.selectMonthObj)
      const month = parseInt(this.selectMonthObj.month, 10) - 1
      const year = parseInt(this.selectMonthObj.year, 10)
      calendar.toDate({ year, month }).paint()
      console.log(month, year)
    }
  }
}
</script>

<style scoped>
  .weekend-words{
    width: 31px;
    height: 30px;
    font-size: 32px;
    font-family: PingFang SC;
    font-weight: bold;
    color: rgba(93, 98, 109, 0.5);
    margin: 0 auto;
    position: relative;
    top: -3px;
  }
  .weekend-mask{
    position: absolute;
    top: 0;
    background: rgba(255,255,255,0.4);
    width: 100%;
    height: 100%;
    z-index: 100;
  }
    .blue-bg{
        background-color: #40FFFF!important;
        color: white;
    }
    .green-bg{
        background-color: #95F204!important;
        color: white;
    }
    .task-label{
        font-size: 12px;
        padding: 3px;
        width: 90%;
        margin: 0 auto;
        border-radius: 4px;
        cursor: pointer;
    }
    .calendar-month-container{
        width: 100%;
        box-sizing: border-box;
        border: 1px solid #18234F;
        border-radius: 4px;
        background: rgba(39, 81, 204, 0.2);
      height: 600px;
      overflow: auto;
    }
    .calendar-month-headers-row{
        display: flex;
        color: #000;
        width: 100%;
        flex-wrap: nowrap;
        border-top: 1px solid #2B2F44;
    }
  .calendar-month-headers-row:last-of-type{
      border-top: 1px solid #2B2F44;
      border-bottom: 1px solid #2B2F44;
  }
  .calendar-month-headers-row:first-of-type{
      border-top: 0;
  }
    .calendar-month-headers{
        padding: 11px 111px;
        color: #000;
        font-size: 14px;
    }
    .calendar-month-body , .calendar-month-headers{
        width: 15%;
        position: relative;
    }
    .calendar-month-body{
        padding: 35px 0px;
        border-right: 1px solid #2B2F44;
    }
    .calendar-month-body:last-of-type{
        border: 0;
        /*border-bottom: 1px solid #2B2F44;*/
    }

    .calendar-month-date{
        position: absolute;
        top: 6px;
        left: 6px;
        font-size: 14px;
    }

    .mask-add-count{
        position: absolute;
        top: 6px;
        right: 6px;
        font-size: 14px;
    }
</style>
