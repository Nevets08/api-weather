<template>
  <main>
    <div class="city-container">
      <div class="city">
        <form @submit.prevent="getData">
          <input
            type="text"
            placeholder="Rechercher une ville"
            v-model="city"
          />
        </form>
        <div class="content" v-if="datas">
          <h1>{{ datas.name }}, {{ datas.sys.country }}</h1>
          <p>Météo d'aujourd'hui le : {{ convertTimestampToDate() }}</p>
          <span class="main-temp">{{ simplifyTemp() }}°</span>
          <h2>{{ capitalizeWeather() }}</h2>
        </div>
        
        <div class="error" v-if="this.error">
          <h2>Cette ville n'a pas été trouvé, veuillez réessayer</h2>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
import axios from "axios";

export default {
  name: "App",

  data() {
    return {
      apiKey: "f5e6e99ab9b1096b9b5266990a12dcb3",
      city: "",
      datas: null,
      error: false,
    };
  },

  methods: {
    getData() {
      axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&APPID=${this.apiKey}&lang=fr&units=metric`
        )
        .then((response) => {
          this.datas = response.data;
          this.city = "";
          this.error = false;
          console.log(this.datas);
        })
        .catch((error) => {
          this.error = error;
          this.datas = null;
        });
    },

    convertTimestampToDate() {
      const timestamp = this.datas.dt;

      const date = new Date(timestamp * 1000);

      return (
        date.getDate() +
        "/" +
        (date.getMonth() + 1) +
        "/" +
        date.getFullYear() +
        " à " +
        date.getHours() +
        ":" +
        date.getMinutes()
      );
    },

    simplifyTemp() {
      const initTemp = `${this.datas.main.temp}`;

      let tempDigit = initTemp.slice(3);
      let newTemp = initTemp.slice(0, -3);

      Math.floor(tempDigit);

      if (tempDigit <= 50) {
        newTemp;
      } else {
        newTemp++;
      }

      tempDigit = "";

      return newTemp;
    },

    capitalizeWeather() {
      const initWeather = this.datas.weather[0].description;

      const capitalizedWeather =
        initWeather.charAt(0).toUpperCase() + initWeather.slice(1);

      return capitalizedWeather;
    },
  },
};
</script>

<style>
body {
  background: #5d83f2;
  color: #fff;
  margin: 0 8%;
}

main {
  margin: 4vh 0;
}

h2 {
  font-weight: lighter;
}

.main-temp {
  font-size: 50px;
  font-weight: bold;
}

main input {
  border-radius: 20px;
  border: none;
  opacity: 0.8;
  height: 30px;
  width: 320px;
  text-align: center;
  font-size: 16px;
  justify-content: center;
}

.city-container {
  display: flex;
  justify-content: center;
  text-align: center;
}

.city h1 {
  text-align: center;
}

.content {
  margin-top: 5vh;
}

.content h2 {
  margin-top: 10px;
}

#app {
  font-family: Poppins, Avenir, Arial, sans-serif;
}
</style>
