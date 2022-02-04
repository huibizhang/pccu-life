<template>
  <div class="w-screen h-screen flex flex-col bg-gray-900 select-none">
    <NavBar />
    <div class="flex-1 flex flex-col p-2 gap-3 overflow-auto">
      <WeatherCard
        v-for="w in weather.filter(w=>w.full)"
        :key="w.Location"
        v-bind="w"
      />
      <WeatherCardCompact
        v-for="w in weather.filter(w=>!w.full)"
        :key="w.Location"
        v-bind="w"
      />
      <img
        src="http://140.137.99.95:8008"
        class="aspect-video object-cover"
      />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import NavBar from "./components/NavBar.vue";
import WeatherCard from "./components/WeatherCard.vue";
import WeatherCardCompact from "./components/WeatherCardCompact.vue";

export default {
  data() {
    return {
      weather: []
    }
  },
  mounted() {
    const _me = this
    axios({
      method: 'get',
      url: 'https://api.pccu.edu.tw/public/weather.json',
    })
    .then(function (response) {
      _me.weather = response.data
      _me.weather.forEach((e,index) => {
        e["full"] = (index===0)
      })
      console.log(_me.weather)
    })
  },
  components: {
    NavBar,
    WeatherCard,
    WeatherCardCompact
  }
}
</script>