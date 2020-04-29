<template>
  <div
    id="app"
    :class="typeof temperatura.main != 'undefined' && temperatura.main.temp > 16 ? 'warm' : ''"
  >
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Busque..."
          v-model="busca"
          @keypress="busqueTemperatura"
        />
      </div>

      <div class="weather-wrap" v-if="typeof temperatura.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ temperatura.name }}, {{ temperatura.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(temperatura.main.temp) }}°c</div>
          <div class="weather">{{ temperatura.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import { apiKey, baseUrl } from "../../.env.js";

export default {
  name: "App",
  data() {
    return {
      api_key: apiKey,
      url_base: baseUrl,
      busca: "",
      temperatura: {}
    };
  },
  methods: {
    busqueTemperatura(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}&q=${this.busca}&units=metric&appid=${this.api_key}`
        )
          .then(response => {
            return response.json();
          })
          .then(this.setResultados);
      }
    },
    setResultados(resultado) {
      this.temperatura = resultado;
    },
    dateBuilder() {
      let d = new Date();
      let months = [
        "janeira",
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
        "dezembro"
      ];
      let days = [
        "Domigo",
        "Segunda-feira",
        "Terça-feira",
        "Quarta-feira",
        "Quinta-feira",
        "Sexta-feira",
        "Sábado"
      ];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} de ${year}`;
    }
  }
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "montserrat", sans-serif;
}

#app {
  background-image: url("../../assets/cold-bg.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4;
}

#app.warm {
  background-image: url("../../assets/warm-bg.jpg");
}

main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
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

  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.15);

  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
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
  color: white;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(255, 255, 255, 0.25);
}
.weather-box .weather {
  color: white;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
