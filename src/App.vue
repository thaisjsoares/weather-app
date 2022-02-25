<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp < 16 ? 'cold' : ''">
    <main>
      <div class="glass">
        <div class="search-box">
          <input
            type="text"
            class="search-bar"
            placeholder="Buscar por cidade"
            v-model="query"
            @keypress.enter="fetchWeather"
          />
        </div>

        <div v-if="loading" class="lds-spinner">
          <div></div>
          <div></div>
          <div></div>
          <div></div>
          <div></div>
          <div></div>
          <div></div>
          <div></div>
          <div></div>
          <div></div>
          <div></div>
          <div></div>
        </div>

        <div class="weather-wrap" v-if="has_data && typeof weather.main != 'undefined'">
          <div class="location-box">
            <div class="location">
              {{ weather.name }}, {{ weather.sys.country }}
            </div>
            <div class="date">{{ dateBuilder() }}</div>
          </div>

          <div class="weather-box">
            <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
            <div class="weather">{{ weather.weather[0].description }}</div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",

  data() {
    return {
      query: "",
      api_key: "6e59f65d5418d97433d47e306a7daf2c",
      url_base: "https://api.openweathermap.org/data/2.5/",
      weather: {},
      loading: false,
      has_data: false,
    };
  },

  methods: {
    async fetchWeather() {
      this.loading = true;
      this.has_data = false;
      try {
        const res = await fetch(`${this.url_base}weather?q=${this.query}
                                 &units=metric&lang=pt_br&appid=${this.api_key}`);
        this.weather = await res.json();
        this.loading = false;
        this.has_data = true;
        this.query = "";
      } catch (error) {
        console.log(error);
        this.loading = false;
      }
    },
    setResults(results) {
      this.weather = results;
    },
    dateBuilder() {
      let d = new Date();
      let months = [
        "janeiro",
        "fevereiro",
        "março",
        "abril",
        "maio",
        "junho",
        "julho",
        "agosto",
        "setembro",
        "outubro",
        "novembro",
        "dezembro",
      ];
      let days = [
        "Domingo",
        "Segunda-feira",
        "Terça-feira",
        "Quarta-feira",
        "Quinta-feira",
        "Sexta-feira",
        "Sábado",
      ];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let current_year = d.getFullYear();

      return `${day}, ${date} de ${month} de ${current_year}`;
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body,
input {
  font-family: "Montserrat", sans-serif;
}

main {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.5)
  );
}

.glass {
  width: 450px;
  height: 600px;
  background-color: rgba(255, 255, 255, 0.15);
  backdrop-filter: blur(5px);
  text-align: center;
}

#app {
  background-image: url("./assets/images/hot.jpg");
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center;
}

#app.cold {
  background-image: url("./assets/images/cold.jpg");
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center;
}



.search-box {
  text-align: center;
  font-size: 20px;
}

.search-box .search-bar {
  margin: 75px auto;
  padding: 10px 15px;
  width: 300px;
  height: 50px;
  font-size: 16px;
  appearance: none;
  border: none;
  outline: none;
  background-color: rgba(236, 236, 236, 1);
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  border-radius: 5px;
}

.location-box {
  margin-top: 1em;
  text-align: center;
}

.location {
  font-family: "Playfair Display", serif;
  font-size: 52px;
  color: #ececec;
  font-style: italic;
  text-shadow: 1px 2px rgba(0, 0, 0, 0.25);
  margin-bottom: 0.3em;
}

.date, .weather {
  color: #ececec;
  font-size: 20px;
  font-weight: 500;
  text-shadow: 1px 2px rgba(0, 0, 0, 0.35);
}

.weather-box {
  margin-top: 3em;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.temp {
  padding: 15px;
  font-size: 80px;
  font-weight: 600;
  color: rgba(236, 236, 236, 1);
}

.weather {

}

.lds-spinner {
  color: official;
  display: inline-block;
  position: relative;
  width: 80px;
  height: 80px;
}

.lds-spinner div {
  transform-origin: 40px 40px;
  animation: lds-spinner 1.2s linear infinite;
}

.lds-spinner div:after {
  content: " ";
  display: block;
  position: absolute;
  top: 3px;
  left: 37px;
  width: 6px;
  height: 18px;
  border-radius: 20%;
  background: #fff;
}

.lds-spinner div:nth-child(1) {
  transform: rotate(0deg);
  animation-delay: -1.1s;
}

.lds-spinner div:nth-child(2) {
  transform: rotate(30deg);
  animation-delay: -1s;
}

.lds-spinner div:nth-child(3) {
  transform: rotate(60deg);
  animation-delay: -0.9s;
}

.lds-spinner div:nth-child(4) {
  transform: rotate(90deg);
  animation-delay: -0.8s;
}

.lds-spinner div:nth-child(5) {
  transform: rotate(120deg);
  animation-delay: -0.7s;
}

.lds-spinner div:nth-child(6) {
  transform: rotate(150deg);
  animation-delay: -0.6s;
}

.lds-spinner div:nth-child(7) {
  transform: rotate(180deg);
  animation-delay: -0.5s;
}

.lds-spinner div:nth-child(8) {
  transform: rotate(210deg);
  animation-delay: -0.4s;
}

.lds-spinner div:nth-child(9) {
  transform: rotate(240deg);
  animation-delay: -0.3s;
}

.lds-spinner div:nth-child(10) {
  transform: rotate(270deg);
  animation-delay: -0.2s;
}

.lds-spinner div:nth-child(11) {
  transform: rotate(300deg);
  animation-delay: -0.1s;
}

.lds-spinner div:nth-child(12) {
  transform: rotate(330deg);
  animation-delay: 0s;
}

@keyframes lds-spinner {
  0% {
    opacity: 1;
  }
  100% {
    opacity: 0;
  }
}

@media (max-width: 600px)
{
  main {
    height: 93vh;
  }

  .glass {
    width: 300px;
  }

  .search-box .search-bar {
    width: 250px;
    box-shadow: 0px 0px 4px rgba(0, 0, 0, 0.25);
  }

  .location {
    font-size: 40px;
  }
}
</style>
