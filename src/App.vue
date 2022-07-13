<template>
  <div id="app" :class=" typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <!-- typeof != "undefined" para que pueda mostrar la informacion -->
    <main class="container">
      <h1>Weather App</h1>

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
        fetch(
          `${this.url}weather?q=${this.buscar}&units=metric&APPID=${this.key}`
        )
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
@import url("https://fonts.googleapis.com/css2?family=Ubuntu&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "Ubuntu", sans-serif;
}

#app {
  background: linear-gradient(to right, #00b4db, #0083b0);
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm {
  background: linear-gradient(to right, #fdc830, #f37335);
}

main {
  min-height: 100vh;
  padding: 25px;
}

h1 {
  display: flex;
  justify-content: center;
  align-items: center;
  color: #fff;
}

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
</style>