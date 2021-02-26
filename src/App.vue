<template>
  <v-app>
    <div id="app">
      <div style="border:1px solid #9a9a9a;width:500px;margin:200px">
        <div class="d-flex align-center cyan accent-2 pl-3">
          <p class="ma-0 pa-0 font-weight-black blue-grey--text text--darken-3">年：</p>
          <v-icon
            @click="actYear(false)"
            class="fa fa-chevron-left mr-5"
            dense
            size="14"
          >
          </v-icon>
          <p class="ma-0 pa-0 blue-grey--text text--darken-2">{{ year }}</p>
          <v-icon
            @click="actYear(true)"
            class="fa fa-chevron-right ml-5"
            dense
            size="14"
          ></v-icon>
        </div>
        <div class="d-flex align-center teal accent-2 pl-3 mt-1">
          <p class="ma-0 pa-0 font-weight-black  ">月：</p>
          <v-icon
            @click="actMonth(false)"
            class="fa fa-chevron-left mr-5"
            dense
            size="14"
          ></v-icon>
          <p class="ma-0 pa-0 blue-grey--text text--darken-2">{{ month }}</p>
          <v-icon
            @click="actMonth(true)"
            class="fa fa-chevron-right ml-5"
            dense
            size="14"
          ></v-icon>
        </div>
        <div style="width:100%" class="d-flex text-center mt-2">
          <p
            v-for="(week, index) in weekList"
            :key="index"
            class="ml-2 blue lighten-4 grey--text"
            style="width:12%"
          >
            {{ week }}
          </p>
        </div>
        <div style="width:500px" class="d-flex flex-wrap text-center blue-grey--text text--darken-2">
          <p
            class="ml-2 grey lighten-4"
            style="width:12%"
            v-for="i in firstMonthDay"
            :key="i"
          ></p>
          <template v-if="monthFlag == 'dan'">
            <p
              v-for="(lastMonthday, i) in danshuMonthList"
              :key="i + 'a'"
              class="ml-2"
              style="width:12%"
              :class="isToday(lastMonthday)?'pink lighten-4':'grey lighten-4'"
            >
              <span > {{ lastMonthday }}</span>
            </p>
          </template>
          <template v-else-if="monthFlag == 'shuang'">
            <p
              v-for="(lastMonthday, i) in shuangshuMonthList"
              :key="i + 'b'"
              class="ml-2"
              style="width:12%"
              :class="isToday(lastMonthday)?'pink lighten-4':'grey lighten-4'"
            >
              <span>{{ lastMonthday }}</span>
            </p>
          </template>

          <template v-else>
            <p
              v-for="(lastMonthday, i) in twoMonthList"
              :key="i + 'c'"
              class="ml-2"
              style="width:12%"
              :class="isToday(lastMonthday)?'pink lighten-4':'grey lighten-4'"
            >
              <span >{{ lastMonthday }}</span>
            </p>
          </template>
        </div>
      </div>
    </div>
  </v-app>
</template>

<script>
export default {
  name: "App",
  components: {},
  data() {
    return {
      weekList: ["周一", "周二", "周三", "周四", "周五", "周六", "周日"],
      firstMonthDay: 0,
      month: new Date().getMonth() + 1,
      year: new Date().getFullYear(),
      day: new Date().getDate(),
      danshuMonthList: [],
      shuangshuMonthList: [],
      twoMonthList: [],
      monthFlag: "",
    };
  },
  created() {
    this.initDate();
    this.firstMonthDayCom();
  },
  methods: {
    initDate() {
      let danshuMonth =
        this.month == 1 ||
        this.month == 3 ||
        this.month == 5 ||
        this.month == 7 ||
        this.month == 8 ||
        this.month == 10 ||
        this.month == 12;
      let shuangshuMonth =
        this.month == 4 ||
        this.month == 6 ||
        this.month == 9 ||
        this.month == 11;

      if (danshuMonth) {
        this.monthFlag = "dan";
        for (let index = 1; index <= 31; index++) {
          this.danshuMonthList.push(index);
        }
      } else if (shuangshuMonth) {
        this.monthFlag = "shuang";
        for (let index = 1; index <= 30; index++) {
          this.shuangshuMonthList.push(index);
        }
      } else {
        this.monthFlag = "two";
        if (this.year % 4 == 0&&this.year%100!==0) {
          for (let index = 1; index <= 29; index++) {
            this.twoMonthList.push(index);
          }
        } else {
          for (let index = 1; index <= 28; index++) {
            this.twoMonthList.push(index);
          }
        }
      }
    },
    firstMonthDayCom() {
      let date = this.year + "/" + this.month;
      console.log(date);
      let myDate = new Date(date);
      this.firstMonthDay = myDate.getDay();
      switch (this.firstMonthDay) {
        case 0:
          this.firstMonthDay = 6;
          break;
        case 1:
          this.firstMonthDay = 0;
          break;
        case 2:
          this.firstMonthDay = 1;
          break;
        case 3:
          this.firstMonthDay = 2;
          break;
        case 4:
          this.firstMonthDay = 3;
          break;
        case 5:
          this.firstMonthDay = 4;
          break;
        case 6:
          this.firstMonthDay = 5;
          break;

        default:
          break;
      }
      console.log(this.firstMonthDay);
    },
    actMonth(flag) {
      if (flag) {
        this.month++;
        if (this.month > 12) {
          this.month = 1;
        }
      } else {
        this.month--;
        if (this.month < 1) {
          this.month = 12;
        }
      }
      this.danshuMonthList = [];
      this.shuangshuMonthList = [];
      this.twoMonthList = [];
      this.firstMonthDayCom();
      this.initDate();
    },
    actYear(flag) {
      if (flag) {
        this.year += 1;
      } else {
        this.year -= 1;
      }
      this.danshuMonthList = [];
      this.shuangshuMonthList = [];
      this.twoMonthList = [];
      this.firstMonthDayCom();
      this.initDate();
    },
    isToday(day){
    return day==this.day&this.month==(new Date().getMonth() + 1)&this.year==new Date().getFullYear()
    }
  },
};
</script>

<style></style>
