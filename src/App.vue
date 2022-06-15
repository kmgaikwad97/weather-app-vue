<template>
  <div id="main">
    <div class="container">
      <h1 class="title text-center">Weather in</h1>
      <form class="search-location" v-on:submit.prevent="getWeather">
        <input
          type="text"
          placeholder="What City ?"
          v-model="citySearch"
          autocomplete="off"
          class="data form-control text-muted form-rounded p-4 shadow-sm"
        />
      </form>

      <div class="card rounded my-3 shadow-lg back-card overflow-hidden">
        <!-- Top of card starts here -->
        <div class="card-top text-center" style="margin-bottom: 15rem">
          <div class="city-name my-3">
            <p>{{ weather.cityName }}</p>
            <span>...</span>
            <p class="">{{ weather.country }}</p>
          </div>
        </div>

        <!-- Top of card ends here -->

        <!-- Card middle body, card body used cos i want to just update the inner -->

        <div class="card-body">
          <!-- card middle starts here -->
          <div class="card-mid">
            <div class="row">
              <div class="col-12 text-center temp">
                <span>{{ weather.temperature }}&deg;C</span>
                <p class="my-4">{{ weather.description }}</p>
              </div>
            </div>
            <div class="row">
              <div class="col d-flex justify-content-between px-5 mx-5">
                <p>
                  <img src="./assets/down.svg" alt="" />
                  {{ weather.lowTemp }}&deg;C
                </p>
                <p>
                  <img src="./assets/up.svg" alt="" />
                  {{ weather.highTemp }}&deg;C
                </p>
              </div>
            </div>
          </div>
          <!-- Card middle ends here -->

          <!-- Card bottom starts here -->

          <div class="card-bottom px-5 py-4 row">
            <div class="col text-center">
              <p>{{ weather.feelsLike }}&deg;C</p>
              <span>Feels Like</span>
            </div>
            <div class="col text-center">
              <p>{{ weather.humidity }}</p>
              <span>Humidity</span>
            </div>
          </div>

          <!-- Card bottom ends here -->
        </div>
      </div>
    </div>
  </div>
</template>


<script>
export default {
  data() {
    return {
      isDay: true,
      weather: {
        cityName: "Gwarinpa",
        country: "NG",
        temperature: 12,
        description: "Clouds Everywhere",
        lowTemp: "19",
        highTemp: "30",
        feelsLike: "20",
        humidity: "55",
      },
    };
  },
  methods: {
    getWeather: async function () {
      console.log(this.citySearch);
      const key = "c675139613f3e634184dbe6106853b63";
      // const baseURL =
      //   "https://api.openweathermap.org/data/2.5/weather?q={this.citySearch}&appid=${key}&units=metric";
      const baseURL = `https://api.openweathermap.org/data/2.5/weather?q=${this.citySearch}&appid=${key}&units=metric`;

      // key="1600d99f62359c3b18137df08210c441"
      // keyname=kshitij

      const response = await fetch(baseURL);
      const data = await response.json();
      console.log(data);
      this.citySearch = "";
      this.weather.cityName = data.name;
      this.weather.country = data.sys.country;
      this.weather.temperature = Math.round(data.main.temp);
      this.weather.description = data.weather[0].description;
      this.weather.lowTemp = Math.round(data.main.temp_min);
      this.weather.highTemp = Math.round(data.main.temp_max);
      this.weather.feelsLike = Math.round(data.main.feels_like);
      this.weather.humidity = Math.round(data.main.humidity);

      const timeOfDay = data.weather[0].icon;

      if (timeOfDay.includes("n")) {
        this.isDay = false;
      } else {
        this.isDay = true;
      }
    },
  },
};
</script>

<style>
@import "./assets/custom.css";
</style>
