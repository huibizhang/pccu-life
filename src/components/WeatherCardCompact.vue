<template>
  <div class="bg-emerald-600 flex-none text-lg flex justify-evenly items-center p-3">
    <div class="flex flex-col gap-1 justify-center items-center">
      <span class="text-emerald-900 font-bold">觀測地點</span>
      <span class="text-gray-200">{{Location}}</span>
    </div>
    <div class="flex flex-col gap-1 justify-center items-center">
      <span class="text-emerald-900 font-bold">更新時間</span>
      <span class="text-gray-200">{{ timeago(new Date(UpdateTime)) }}</span>
    </div>
    <div class="text-emerald-900 font-bold">{{WeatherDesciption}}</div>
    <div class="flex font-bold justify-center items-end gap-1 text-gray-200">
      <span class="text-3xl">{{Tempature}}</span>
      <span class="text-2xl">℃</span>
    </div>
  </div>
</template>

<script>
export default {
  props: [
    "Location",
    "Tempature",
    "Humidity",
    "WindDirection",
    "WindSpeed",
    "Atmosph",
    "RainFall",
    "UpdateTime",
    "InfoSource",
    "WeatherDesciption",
    "full",
  ],
  mounted(){

  },
  methods: {
    timeago(dateTimeStamp){   //dateTimeStamp是一個時間毫秒，註意時間戳是秒的形式，在這個毫秒的基礎上除以1000，就是十位數的時間戳。13位數的都是時間毫秒。
      var minute = 1000 * 60;      //把分，時，天，周，半個月，一個月用毫秒表示
      var hour = minute * 60;
      var day = hour * 24;
      var week = day * 7;
      var halfamonth = day * 15;
      var month = day * 30;
      var now = new Date().getTime();   //獲取當前時間毫秒
      console.log(now)
      var diffValue = now - dateTimeStamp;//時間差

      var result = ""

      if(diffValue < 0){
          return;
      }
      var minC = diffValue/minute;  //計算時間差的分，時，天，周，月
      var hourC = diffValue/hour;
      var dayC = diffValue/day;
      var weekC = diffValue/week;
      var monthC = diffValue/month;
      if(monthC >= 1 && monthC <= 3){
          result = " " + parseInt(monthC) + "月前"
      }else if(weekC >= 1 && weekC <= 3){
          result = " " + parseInt(weekC) + "周前"
      }else if(dayC >= 1 && dayC <= 6){
          result = " " + parseInt(dayC) + "天前"
      }else if(hourC >= 1 && hourC <= 23){
          result = " " + parseInt(hourC) + "小時前"
      }else if(minC >= 1 && minC <= 59){
          result =" " + parseInt(minC) + "分鐘前"
      }else if(diffValue >= 0 && diffValue <= minute){
          result = "剛剛"
      }else {
          var datetime = new Date();
          datetime.setTime(dateTimeStamp);
          var Nyear = datetime.getFullYear();
          var Nmonth = datetime.getMonth() + 1 < 10 ? "0" + (datetime.getMonth() + 1) : datetime.getMonth() + 1;
          var Ndate = datetime.getDate() < 10 ? "0" + datetime.getDate() : datetime.getDate();
          var Nhour = datetime.getHours() < 10 ? "0" + datetime.getHours() : datetime.getHours();
          var Nminute = datetime.getMinutes() < 10 ? "0" + datetime.getMinutes() : datetime.getMinutes();
          var Nsecond = datetime.getSeconds() < 10 ? "0" + datetime.getSeconds() : datetime.getSeconds();
          result = Nyear + "-" + Nmonth + "-" + Ndate
      }
      return result;
    }
  }
}
</script>