<template>
  <!-- typeof != "undefined" para que pueda mostrar la informacion -->
  <div id="app" :class=" typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <main class="container">
      <h1 class="titulo">Weather App</h1>

      <div class="search-box">
        <input type="text" class="form-control search-bar" placeholder="Search..." v-model.trim="buscar" @keypress="fetchWeather" />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
          <div class="date">{{ fecha() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import Swal from "sweetalert2"; // npm

export default {
  name: "app",
  data() {
    return {
      key: "333d98cc99af02c7246da1e7ff0fdd56",
      url: "https://api.openweathermap.org/data/2.5/",
      buscar: "",
      weather: {},
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(`${this.url}weather?q=${this.buscar}&units=metric&APPID=${this.key}`)
          .then((res) => {
            return res.json();
          })
          .then(this.setResults);
      }
    },

    setResults(results) {
      if (this.buscar !== "") {
        this.weather = results;
      } else {
        Swal.fire({
          title: "Error!",
          text: "add valid country or city",
          icon: "error",
        });
      }
      this.buscar = "";
    },

    fecha() {
      const d = new Date();
      const months = ["January","February","March","April","May","June","July","August","September","October","November","December",];
      const days = ["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday",];
      const day = days[d.getDay()];
      const date = d.getDate();
      const month = months[d.getMonth()];
      const year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    },
  },
};
</script>

<style>
#app, #app.warm {animation: var(--animacion-fondo);}:root {--animacion-fondo: bg-animado 5s linear infinite alternate;--tamaño-fondo: 200% 200%;}* {margin: 0;padding: 0;box-sizing: border-box;}body {font-family: Anton, sans-serif;}@keyframes bg-animado {from {background-position: 0, 0;}to {background-position: 100% 100%;}}#app {background: linear-gradient(to right, #00c6ff, #0072ff);background-size: cover;background-position: bottom;transition: 0.4s;background-size: var(--tamaño-fondo);}#app.warm {background: linear-gradient(to right, #fd1d1d, #fcb045);background-size: var(--tamaño-fondo);}main {min-height: 100vh;padding: 25px;}.titulo {text-align: center;color: transparent;text-transform: uppercase;font-weight: bold;font-size: 3.5rem;letter-spacing: 3px;background: url("../src/assets/titulo-imagen.webp");background-size: cover;background-position: top;background-clip: text;-webkit-background-clip: text;-webkit-text-fill-color: transparent;}.search-box {width: 100%;margin-bottom: 30px;}.search-box .search-bar {display: block;width: 100%;padding: 15px;color: #313131;font-size: 20px;appearance: none;border: none;outline: 0;background: rgba(255, 255, 255, 0.5);box-shadow: 0 0 8px rgba(0, 0, 0, 0.25);transition: 0.4s;}.search-box .search-bar:focus {box-shadow: 0 0 16px rgba(0, 0, 0, 0.25);background-color: rgba(255, 255, 255, 0.75);}.location-box .location {color: #fff;font-size: 32px;font-weight: 500;text-align: center;text-shadow: 1px 3px rgba(0, 0, 0, 0.25);}.weather-box .temp, .weather-box .weather {color: #fff;text-shadow: 3px 6px rgba(0, 0, 0, 0.25);}.location-box .date {color: #fff;font-size: 20px;font-weight: 300;font-style: italic;text-align: center;}.weather-box {text-align: center;}.weather-box .temp {display: inline-block;padding: 10px 25px;font-size: 102px;font-weight: 900;background-color: rgba(255, 255, 255, 0.25);border-radius: 16px;margin: 30px 0;box-shadow: 3px 6px rgba(0, 0, 0, 0.25);}.weather-box .weather {font-size: 48px;font-weight: 700;font-style: italic;}@media (min-width: 1280px) {.titulo {font-size: 6rem;}}
</style>