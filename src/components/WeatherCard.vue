<template>
  <div class="bg-emerald-500 flex-none text-lg">
    <div>
      <div class="w-full flex justify-evenly items-center p-4 gap-8">
        <div class="w-20 h-20 text-white flex-none flex justify-center items-center">
          <!-- 雲朵 -->
          <!-- <svg xmlns="http://www.w3.org/2000/svg" class="h-16 w-16" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 15a4 4 0 004 4h9a5 5 0 10-.1-9.999 5.002 5.002 0 10-9.78 2.096A4.001 4.001 0 003 15z" />
          </svg> -->
          <img
            :src="`weather_icons/${getWetherIcon(WeatherDesciption)}.png`"
            class="w-full h-full object-fit"
          />
        </div>
        <div class="flex justify-center items-center">
          <div class="flex flex-col gap-1">
            <div class="text-emerald-800 font-bold">{{WeatherDesciption}}</div>
            <div class="flex font-bold justify-center items-end gap-1 text-white">
              <span class="text-5xl">{{Tempature}}</span>
              <span class="text-2xl">℃</span>
            </div>
          </div>
        </div>
      </div>
      <div class="flex justify-between px-8 pb-3">
        <div class="flex flex-col gap-0.5 justify-center items-center">
          <span class="text-emerald-800 font-bold">相對溼度</span>
          <span class="text-white text-2xl font-bold">{{Humidity}}%</span>
        </div>
        <div class="flex flex-col gap-0.5 justify-center items-center">
          <span class="text-emerald-800 font-bold">現在風速</span>
          <span class="text-white text-2xl font-bold">{{WindSpeed}}m/s</span>
        </div>
        <div class="flex flex-col gap-0.5 justify-center items-center">
          <span class="text-emerald-800 font-bold">累積雨量</span>
          <span class="text-white text-2xl font-bold">{{RainFall}}mm</span>
        </div>
      </div>
    </div>
    <div class="bg-emerald-800 flex justify-evenly p-4">
      <div class="flex flex-col gap-1 justify-center items-center">
        <span class="text-orange-400 font-bold">觀測地點</span>
        <span class="text-white">{{Location}}</span>
      </div>
      <div class="flex flex-col gap-1 justify-center items-center">
        <span class="text-orange-400 font-bold">更新時間</span>
        <span class="text-white">{{ timeago(new Date(UpdateTime)) }}</span>
      </div>
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
    },
    getWetherIcon (weatherDescription) {
      if (weatherDescription.indexOf('雨') != -1) {
          if (weatherDescription.indexOf('雷') != -1 && weatherDescription.indexOf('霧') != -1) {
              return "fog_thunder";
          }
          if (weatherDescription.indexOf('雷') != -1) {
              return "cloudy_rain_thunder";
          }
          if (weatherDescription.indexOf('霧') != -1) {
              return "fog_rain";
          }

          if (weatherDescription.indexOf('晴') != -1) {
              return "sunny_cloudy_rain";
          }

          return "cloudy_rain";
      }

      if (weatherDescription.indexOf('霧') != -1) {
          if (weatherDescription.indexOf('晴') != -1) {
              return "sunny_fog";
          }
          return "cloudy_fog";
      }

      if (weatherDescription.indexOf('晴') != -1) {
          if (weatherDescription.indexOf('雲') != -1) {
              return "sunny_cloudy";
          }
          return "sunny";
      }

      if (weatherDescription.indexOf('多雲') != -1) {
          return "muti_cloudy";
      }

      if (weatherDescription.indexOf('雪') != -1) {
          return "cloudy_snow";
      }

      return "cloudy";
    }
  }
}
</script>