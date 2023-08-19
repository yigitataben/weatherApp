<template>
  <div id="app">
    <main>
      <div class="searchBox">
        <div class="searchBox">
          <input
              type="text"
              class="searchBar"
              placeholder="Search..."
              v-model = "query"
              @keyup.enter="getWeather"/>
        </div>

        <div class="weatherWrap" v-if="typeof weather.main != 'undefined'">
          <div class="locationBox">
            <div class="location">{{weather.name}}, {{weather.sys.country}}</div>
            <div class="date">{{moment(new Date()).format('MMMM Do YYYY, h:mm:ss a')}}</div>
          </div>

          <div class="weatherBox">
            <div class="temp">{{weather.main.temp}}°c</div>
            <div class="weather">{{weather.weather[0].main}}</div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import Vue from 'vue'
import moment from 'moment'

Vue.prototype.moment = moment

export default {
  name: 'app',
  data() {
    return {
      apiKey: '7fb30de8c3af5da82f0ed611feff4004',
      urlBaseGeo: 'https://api.openweathermap.org/geo/1.0/',
      urlBaseData: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {},
      lat: 0,
      lon: 0
    }
  },
  methods: {
    async getWeather() {
      if (this.query) {
        const geoResponse = await fetch(`${this.urlBaseGeo}direct?q=${this.query}&appid=${this.apiKey}`);
        const geoData = await geoResponse.json();

        if (geoData.length > 0) {
          this.lat = geoData[0].lat;
          this.lon = geoData[0].lon;

          const weatherResponse = await fetch(`${this.urlBaseData}weather?lat=${this.lat}&lon=${this.lon}&units=metric&appid=${this.apiKey}`);
          this.weather = await weatherResponse.json();
        }
      }
    },
  }
}
</script>

<style>

@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@900&display=swap');
  *{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  body {
    font-family: 'Montserrat', sans-serif;
  }

  #app{
    background-image: url("./assets/background.jpg");
    background-size: cover;
    background-position: bottom;
    transition: 0.4s;
  }

  main{
    min-height: 100vh;
    padding: 25px;

    background-image: linear-gradient(to bottom, rgba(0,0,0,0.25), rgba(0,0,0,0.75)) ;
  }
  .searchBox {
    width: 100%;
    margin-bottom: 30px;
  }
  .searchBox .searchBar {
    display: block;
    width: 100%;
    padding: 15px;

    color: #313131;
    font-size: 20px;

    appearance: none;
    border: none;
    outline: none;
    box-shadow: 0 0 8px rgba(0, 0, 0, 0.25);
    background: rgba(255, 255, 255, 0.15) none;
    border-radius: 0 16px 0 16px;
    transition: 0.4s;
  }

  .searchBox .searchBar:focus {
    box-shadow: 0 0 16px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.5);
    border-radius: 16px 16px 16px 16px;
  }

  .locationBox .location {
    color: #FFFF;
    font-size: 32px;
    font-weight: 500;
    text-align: center;
    text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  }

  .locationBox .date {
    color: #FFFF;
    font-size: 20px;
    font-weight: 300;
    font-style: normal;
    text-align: center;
  }

  .weatherBox {
    text-align: center;
  }

  .weatherBox .temp {
    display: inline-block;
    padding: 10px 25px;
    color: #FFF;
    font-size: 100px;
    font-weight: 900;

    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.25);
    border-radius: 16px;
    margin: 30px 0;

    box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  }

.weatherBox .weather {
  color: white;
  font-size: 48px;
  font-weight: 700;
  font-style: normal;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
