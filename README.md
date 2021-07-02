# PCCanlendar
PC端的日历组件，用于任务排期日历展示。
使用方法：
```javascript
 <Calendar
    :work-memoir-list="MockData"
    :select-month-obj="selectMonthObj"
/>

data() {
    return {
      MockData: workList,
      selectMonthObj: {
        year: new Date().getFullYear(),
        month: `${new Date().getMonth() + 1 > 10 ? (new Date().getMonth() + 1) : '0' + (new Date().getMonth() + 1)}`
      }
    }
  }
  
  export const workList = [{"id":74,"actuallyTime":"08:20-08:50","actuallyFinishTime":"2021-07-02 15:51:08","createTime":"2021-07-02 15:51:08","postTaskVOList":[{"id":270,"postId":1,"level":1,"parentId":0,"taskDesc":"小李测试一","createTime":"2021-06-17 17:27:06","createPersonnel":null,"updateTime":"2021-06-17 17:27:06","updatePersonnel":null,"deleteTime":null,"deletePersonnel":null,"deleteFlag":0},{"id":271,"postId":1,"level":2,"parentId":270,"taskDesc":"小李测试二","createTime":"2021-06-17 17:27:06","createPersonnel":null,"updateTime":"2021-06-17 17:27:06","updatePersonnel":null,"deleteTime":null,"deletePersonnel":null,"deleteFlag":0},{"id":272,"postId":1,"level":3,"parentId":271,"taskDesc":"小李测试三","createTime":"2021-06-17 17:27:06","createPersonnel":null,"updateTime":"2021-06-17 17:27:06","updatePersonnel":null,"deleteTime":null,"deletePersonnel":null,"deleteFlag":0}]}]
```
![日历例子](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f8764beb09e24d1c821a2dbee3663b39~tplv-k3u1fbpfcp-watermark.image)
