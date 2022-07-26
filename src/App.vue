<template>
  <!-- typeof != "undefined" para que pueda mostrar la informacion -->
  <div id="app" :class=" typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <main class="container">
      <!--<div class="contenedor-video">
        <video autoplay muted preload loop src="../src/assets/weather-titulo.mp4" type="video/mp4" />
        <h1 class="titulo">Weather App</h1>
      </div>-->

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
      const months = ["January","February","March","April","May","June","July","August","September","October","November","December"];
      const days = ["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"];
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
@import url("https://fonts.googleapis.com/css2?family=Anton&display=swap");

:root {
  --animacion-fondo: bg-animado 5s linear infinite alternate;
  --tamaño-fondo: 200% 200%;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "Anton", sans-serif;
}

@keyframes bg-animado {
  from {
    background-position: 0, 0;
  }
  to {
    background-position: 100% 100%;
  }
}

#app {
  background: linear-gradient(to right, #00c6ff, #0072ff);
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
  animation: var(--animacion-fondo);
  background-size: var(--tamaño-fondo);
}

#app.warm {
  background: linear-gradient(to right, #fd1d1d, #fcb045);
  animation: var(--animacion-fondo);
  background-size: var(--tamaño-fondo);
}

main {
  min-height: 100vh;
  padding: 25px;
}

.titulo {
  text-align: center;
  color: transparent;
  text-transform: uppercase;
  font-weight: bold;
  font-size: 3.5rem;
  letter-spacing: 3px;

  /* para el video del titulo */
  /*mix-blend-mode: multiply;*/

  /* imagen en el titulo */
  /*background-image: url("../src/assets/weather-titulo.mp4");*/
  background: url("../src/assets/titulo-gif.gif") no-repeat center center / cover;
  background-size: cover;
  background-position: center;
  background-clip: text;

  /* para que ande en todos los navegadores */
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

/*.contenedor-video {
  position: relative;
}

video {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
}*/

.search-box {
  width: 100%;
  margin-bottom: 30px;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
}

.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

@media (min-width: 1280px) {
  .titulo {
    font-size: 6rem;
  }
}
</style>