<template>
  <view class="u-calendar-box" :id="'u' + _uid" :style="{ height: height + 'rpx', paddingTop: fixdHeaderHeight + 'px', paddingBottom: fixdFooterHeight + 'px' }">
    <view class="u-calendar-fixd">
      <view v-if="tipArr.length != 0" class="u-calendar-tips">
        <view class="u-calendar-tip-item" v-for="(item, index) in tipArr" :key="index" v-text="item.tipName" @click="tipClick(item)"></view>
      </view>
      <view class="u-calendar-week">
        <view class="u-calendar-week-item" v-for="(item, index) in weekTitle" :key="index" v-text="item"> </view>
      </view>
    </view>
    <view class="u-calendar-content">
      <scroll-view class="u-calendar-scroll-box" scroll-y>
        <view class="u-calendar-year-box" v-for="(item, index) in dayDateList" :key="index">
          <view class="u-calendar-month-box" v-for="(mitem, mindex) in item.options.month" :key="mindex">
            <view class="u-calendar-year-tip" v-text="formatYearTitle(item, mitem)"></view>
            <view class="u-calendar-day-box">
              <view class="u-calendar-day-item" :style="{ width: 100 / 7 + '%' }" v-for="(oitem, oindex) in item.options.oneDayWeekList[mindex]" :key="'u' + oindex"></view>
              <view class="u-calendar-day-item" :style="{ width: 100 / 7 + '%' }" :class="{ startDate: startDate(item.year, mitem, ditem), endDate: endDate(item.year, mitem, ditem) }" @click="selectItem(item.year, mitem, ditem)" v-for="(ditem, dindex) in item.options.dayList[mindex]" :key="dindex">
                <view class="u-calendar-day-item-value" :class="{ active: isSelectClass(item.year, mitem, ditem), range: isRange(item.year, mitem, ditem), disabled: isDisabled(item.year, mitem, ditem) }" v-text="ditem"></view>
              </view>
            </view>
          </view>
        </view>
      </scroll-view>
    </view>
    <view v-if="type == 'range'" class="u-calendar-footer">
      <button class="u-calendar-confirm-btn" @click="confirm">确定</button>
    </view>
  </view>
</template>

<script>
/**
 * 组件名称
 * @description 组件注释
 * @tutorial 文档地址
 * @property {属性值的类型} 属性名 属性描述
 * @event {Function} 事件名 事件描述
 * @example <dd-xxx></dd-xxx>
 */
export default {
  name: 'DdCalendar',
  props: {
    type: {
      type: String,
      default: 'date'
    },
    startYear: {
      type: [Number, String],
      default: '',
      required: true
    },
    endYear: {
      type: [Number, String],
      default: '',
      required: true
    },
    height: {
      type: [Number, String],
      default: '1000'
    },
    tipArr: {
      type: Array,
      default: () => [
        {
          tipName: '近24小时',
          tipTime: 24 * 60 * 60 * 1000
        },
        {
          tipName: '近7天',
          tipTime: 7 * 24 * 60 * 60 * 1000
        },
        {
          tipName: '近30天',
          tipTime: 30 * 24 * 60 * 60 * 1000
        },
        {
          tipName: '近90天',
          tipTime: 90 * 24 * 60 * 60 * 1000
        }
      ]
    },
    pickerOptions: {
      type: Object,
      default: () => {
        return {
          disabledDate(time) {
            return time > new Date().getTime()
            // return false
          }
        }
      }
    }
  },
  computed: {
    formatYearTitle(item, mitem) {
      return (item, mitem) => {
        return `${item.year}年${mitem}月`
      }
    },
    isSelectClass(item, mitem, ditem) {
      return (item, mitem, ditem) => {
        let result = this.currentSelect[0].result
        let selectResult = `${item}-${this.dateSeats(mitem)}-${this.dateSeats(ditem)}`
        if (this.type === 'date') {
          if (result === selectResult) {
            return true
          } else {
            return false
          }
        } else {
          if (this.currentSelect[1]) {
            let endResult = this.currentSelect[1].result
            if (result === selectResult || endResult === selectResult) {
              return true
            } else {
              return false
            }
          }
          if (this.currentSelect[0]) {
            if (result === selectResult) {
              return true
            } else {
              return false
            }
          }
        }
      }
    },
    isRange(item, mitem, ditem) {
      return (item, mitem, ditem) => {
        if (this.currentSelect[1]) {
          let selectResult = `${item}-${this.dateSeats(mitem)}-${this.dateSeats(ditem)}`
          let startTime = new Date(this.currentSelect[0].result).getTime()
          let selectTime = new Date(selectResult).getTime()
          let endTime = new Date(this.currentSelect[1].result).getTime()
          if (selectTime > startTime && selectTime < endTime) {
            return true
          } else {
            return false
          }
        }
      }
    },
    startDate(item, mitem, ditem) {
      return (item, mitem, ditem) => {
        if (this.currentSelect.length === 2) {
          let startTime = this.currentSelect[0].result
          let selectResult = `${item}-${this.dateSeats(mitem)}-${this.dateSeats(ditem)}`
          if (startTime === selectResult) {
            return true
          } else {
            return false
          }
        }
      }
    },
    endDate(item, mitem, ditem) {
      return (item, mitem, ditem) => {
        if (this.currentSelect.length === 2) {
          let endTime = this.currentSelect[1].result
          let selectResult = `${item}-${this.dateSeats(mitem)}-${this.dateSeats(ditem)}`
          if (endTime === selectResult) {
            return true
          } else {
            return false
          }
        }
      }
    },
    isDisabled(item, mitem, ditem) {
      return (item, mitem, ditem) => {
        let selectResult = `${item}-${this.dateSeats(mitem)}-${this.dateSeats(ditem)}`
        let sTime = new Date(selectResult.replace(/\-/g, '/')).getTime()
        return this.pickerOptions.disabledDate(sTime)
      }
    }
  },
  data() {
    return {
      weekTitle: ['日', '一', '二', '三', '四', '五', '六'],
      currentDate: '',
      dayDateList: [],
      fixdHeaderHeight: 0,
      fixdFooterHeight: 0,
      currentSelect: [{}]
    }
  },
  methods: {
    initDate() {
      this.getDateDayList()
      this.initContent()
    },
    initContent() {
      this.$nextTick(async() => {
        this.fixdHeaderHeight = await this.getElRect('#u' + this._uid + ' .u-calendar-fixd')
        this.fixdFooterHeight = await this.getElRect('#u' + this._uid + ' .u-calendar-footer')
      })
    },
    tipClick(item) {
      let eTime = new Date()
      let eNum = eTime.getTime()
      let sTime = new Date(eNum - item.tipTime)

      let sYear = sTime.getFullYear()
      let sMonth = sTime.getMonth() + 1
      let sDay = sTime.getDate()
      let sResult = `${sYear}-${this.dateSeats(sMonth)}-${this.dateSeats(sDay)}`
      let sWeek = this.getWeekday(sYear, sMonth, sDay, 'str')

      let eYear = eTime.getFullYear()
      let eMonth = eTime.getMonth() + 1
      let eDay = eTime.getDate()
      let eResult = `${eYear}-${this.dateSeats(eMonth)}-${this.dateSeats(eDay)}`
      let eWeek = this.getWeekday(eYear, eMonth, eDay, 'str')

      let startData = {
        day: sDay, // 选择了哪一天
        month: sMonth, // 选择的月份
        result: sResult, // 选择的日期整体值
        week: sWeek, // 选择日期所属的星期数
        year: sYear // 选择的年份
      }

      let endDate = {
        day: eDay, // 选择了哪一天
        month: eMonth, // 选择的月份
        result: eResult, // 选择的日期整体值
        week: eWeek, // 选择日期所属的星期数
        year: eYear // 选择的年份
      }

      this.$set(this.currentSelect, 0, startData)
      this.$set(this.currentSelect, 1, endDate)
    },
    dateSeats(str) {
      if (str >= 0 && str < 10) {
        str = '0' + str
      }
      return str
    },
    selectItem(year, month, day) {
      if (this.isDisabled(year, month, day)) {
        return
      }
      if (this.type === 'date') {
        let dateData = {
          day: day, // 选择了哪一天
          month: month, // 选择的月份
          result: `${year}-${this.dateSeats(month)}-${this.dateSeats(day)}`, // 选择的日期整体值
          week: this.getWeekday(year, this.dateSeats(month), this.dateSeats(day), 'str'), // 选择日期所属的星期数
          year: year // 选择的年份
        }
        this.$set(this.currentSelect, 0, dateData)
        this.$emit('change', this.currentSelect)
      } else {
        let dateData = {
          day: day, // 选择了哪一天
          month: month, // 选择的月份
          result: `${year}-${this.dateSeats(month)}-${this.dateSeats(day)}`, // 选择的日期整体值
          week: this.getWeekday(year, this.dateSeats(month), this.dateSeats(day), 'str'), // 选择日期所属的星期数
          year: year // 选择的年份
        }
        if (this.currentSelect.length === 2) {
          this.currentSelect.length = 0
          this.$set(this.currentSelect, 0, {})
        }
        if (this.currentSelect.length === 0) {
          this.$set(this.currentSelect, 0, dateData)
        }
        if (this.currentSelect.length === 1) {
          if (!this.currentSelect[0].day) {
            this.$set(this.currentSelect, 0, dateData)
          } else {
            let startTime = new Date(this.currentSelect[0].result).getTime()
            let endTime = new Date(dateData.result).getTime()
            if (endTime > startTime) {
              this.$set(this.currentSelect, 1, dateData)
            }
            if (startTime > endTime) {
              this.$set(this.currentSelect, 1, dateData)
              this.currentSelect.reverse()
            }
            if (startTime === endTime) {
              // 开始时间和结束时间是否一致
              this.$set(this.currentSelect, 0, dateData)
              this.$set(this.currentSelect, 1, dateData)
            }
          }
        }
      }
    },
    getDateDayList() {
      let dateNumber
      if (this.endYear - this.startYear !== 1) {
        dateNumber = this.endYear - this.startYear
      } else {
        dateNumber = 2
      }
      for (let i = 0; i < dateNumber; i++) {
        let dayOptions = {
          dayList: [],
          year: parseInt(this.startYear) + i,
          month: 0,
          oneDayWeekList: []
        }
        for (let j = 0; j < 12; j++) {
          dayOptions.month = j + 1
          dayOptions.dayList.push(this.getMonthDay(dayOptions.year, dayOptions.month))
          dayOptions.oneDayWeekList.push(this.getWeekday(dayOptions.year, dayOptions.month))
        }
        this.dayDateList.push({
          year: dayOptions.year,
          options: dayOptions
        })
      }
    },
    // 一个月有多少天
    getMonthDay(year, month) {
      let days = new Date(year, month, 0).getDate()
      return days
    },
    getWeekday(year, month, day = '01', type = 'num') {
      if (type === 'num') {
        return new Date(Date.parse(year + '/' + month + '/' + day)).getDay()
      } else {
        let week = new Date(Date.parse(year + '/' + month + '/' + day)).getDay()
        return '星期' + ['日', '一', '二', '三', '四', '五', '六'][week]
      }
    },
    // 获取一个目标元素的高度
    getElRect(elClass) {
      return new Promise((resolve, reject) => {
        let query = uni.createSelectorQuery().in(this)
        query
          .select(elClass)
          .fields(
            {
              size: true
            },
            (res) => {
              // 如果节点尚未生成，res值为null，循环调用执行
              if (!res) {
                setTimeout(() => {
                  this.getElRect(elClass)
                }, 10)
                return
              }
              resolve(res.height)
            }
          )
          .exec()
      })
    },
    confirm() {
      if (this.currentSelect.length !== 2) {
        alert('请选择完整日期！')
      } else {
        this.$emit('change', this.currentSelect)
      }
    }
  },
  created() {
    this.initDate()
  }
}
</script>

<style lang="scss" scoped>
.u-calendar-box {
  position: relative;
  background-color: #fff;

  .u-calendar-fixd {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    z-index: 3;
    background-color: #fff;
    box-shadow: 0px 4px 4px 0px rgba(34, 89, 229, 0.05);

    .u-calendar-tips {
      display: flex;
      margin: 10px 15px;
      .u-calendar-tip-item {
        display: flex;
        align-items: center;
        justify-content: center;
        flex: 1;
        font-size: 24rpx;
        font-family: PingFangSC-Regular, PingFang SC;
        font-weight: 400;
        color: #74757a;
        padding: 5px 0;
        border-top: 1px solid #ebecf0;
        border-right: 1px solid #ebecf0;
        border-bottom: 1px solid #ebecf0;
      }
      .u-calendar-tip-item:first-child {
        border-left: 1px solid #ebecf0;
        border-radius: 4px 0 0 4px;
      }
      .u-calendar-tip-item:last-child {
        border-radius: 0px 4px 4px 0;
      }
    }

    .u-calendar-week {
      display: flex;
      padding: 0 22rpx;
      align-items: center;
      // border-bottom: 1px solid $u-border-color;

      .u-calendar-week-item {
        display: flex;
        padding: 16rpx 0;
        align-items: center;
        justify-content: center;
        font-size: 28rpx;
        font-family: PingFangSC-Regular, PingFang SC;
        font-weight: 400;
        color: #111111;
        flex: 1;
      }
    }
  }

  .u-calendar-content {
    height: 714rpx;
    &::-webkit-scrollbar {
      display: none;
    }

    .u-calendar-scroll-box {
      height: 100%;
      .u-calendar-year-box {
        .u-calendar-month-box {
          .u-calendar-year-tip {
            padding: 8rpx 0 8rpx 22rpx;
            font-size: 28rpx;
            font-family: PingFangSC-Regular, PingFang SC;
            font-weight: 400;
            color: #111111;
          }

          .u-calendar-day-box {
            display: flex;
            padding: 0 22rpx;
            flex-wrap: wrap;
            text-align: center;
            .u-calendar-day-item {
              position: relative;
              display: flex;
              align-items: center;
              justify-content: center;
              padding: 16rpx 0;
              font-size: 28rpx;
              font-family: PingFangSC-Regular, PingFang SC;
              font-weight: 400;
              color: #111111;
              z-index: 1;
              .u-calendar-day-item-value {
                display: flex;
                align-items: center;
                justify-content: center;
                width: 72rpx;
                height: 72rpx;
              }
              .u-calendar-day-item-value.range {
                width: 100%;
                background-color: #f5f8fe;
              }
              .u-calendar-day-item-value.active {
                background: #2259e5;
                color: #ffffff;
                border-radius: 4px;
              }
              .u-calendar-day-item-value.disabled {
                color: #abaeba;
                cursor: not-allowed;
              }
            }
            .u-calendar-day-item.startDate::before {
              position: absolute;
              content: '';
              right: 0;
              width: 50%;
              height: calc(100% - 32rpx);
              background-color: #f5f8fe;
              z-index: -1;
            }
            .u-calendar-day-item.endDate::before {
              position: absolute;
              content: '';
              left: 0;
              width: 50%;
              height: calc(100% - 32rpx);
              background-color: #f5f8fe;
              z-index: -1;
            }
            .u-calendar-day-item.startDate.endDate::before {
              content: none;
            }
          }
        }
      }
    }
  }
  .u-calendar-footer {
    position: absolute;
    padding: 22rpx;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: #fff;
    z-index: 3;
    .u-calendar-confirm-btn {
      padding: 6rpx;
      font-size: 24rpx;
      font-family: PingFangSC-Regular, PingFang SC;
      font-weight: 500;
      color: #fff;
      outline: none;
      border: none;
      background: #2259e5;
      border-radius: 4px;
    }
  }
}
</style>