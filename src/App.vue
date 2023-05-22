<template>
  <div class="background" :class="backgroundBuilder()">
    <div class="container mx-auto">
      <header class="py-6 px-4">
        <div class="flex gap-2">
          <input type="text" placeholder="Search the city..." class="w-full border px-4 py-2 rounded" v-model="query"
                 @keydown="fetchWeather">
          <button class="p-2 rounded bg-white" @click="fetchWeather">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none"
                 viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                    d="M15 13l-3 3m0 0l-3-3m3 3V8m0 13a9 9 0 110-18 9 9 0 010 18z"/>
            </svg>
          </button>
        </div>
      </header>
      <main class="text-white flex flex-col gap-4" v-if="typeof weather.main != 'undefined'">
        <div class="location text-center flex flex-col gap-2">
          <h1 class="text-4xl font-bold city">{{ weather.name }}, {{ weather.sys.country }}</h1>
          <h2 class="text-2xl">{{ dateBuilder() }}</h2>
        </div>
        <div class="flex flex-col items-center gap-2">
          <div class="temp p-8 bg-white bg-opacity-60 text-center text-[64px] rounded-2xl w-fit font-bold">
            {{ Math.round(weather.main.temp) }}°C
          </div>
          <div class="weather text-[48px] font-bold">{{ weather.weather[0].main }}</div>
          <div class="hi-low flex gap-4">
            <p><span class="font-bold">High:</span> {{ Math.round(weather.main.temp_max) }}°C</p>
            <p><span class="font-bold">Low:</span> {{ Math.round(weather.main.temp_min) }}°C</p>
          </div>
          <div class="feels-like">
            <span class="font-bold">Feels like:</span> {{ Math.round(weather.main.feels_like) }}°C
          </div>
        </div>
      </main>
      <div>
        <div class="text-center text-white text-2xl" v-if="typeof weather.main == 'undefined'">
          <p>Search for a city to get the weather</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      api_key: '551dc6d09ab66b0c50aa4a3fe8c022fe',
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
    }
  },
  methods: {
    fetchWeather(e) {
      if (e.key === "Enter" || e.type === "click") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
            .then(res => res.json())
            .then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
    },
    dateBuilder() {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`
    },
    backgroundBuilder() {
      let d = new Date();
      let hours = d.getHours();
      if (hours >= 6 && hours < 12) {
        return 'background-day-clear'
      } else if (hours >= 12 && hours < 18) {
        return 'background-afternoon-clear'
      } else {
        return 'background-night-clear'
      }
    }
  }
}
</script>

