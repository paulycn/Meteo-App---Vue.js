<script >
import axios from "axios"

export default {
  data(){
    return {
      city: "",
      error:"",
      info: null
    }
  },
  computed: {
    cityName(){
      return this.city 
    },
    showTemperature(){
      return "Temperature: " + Math.round(this.info.main.temp) + "°"
    },
    showFeelsLike(){
      return "Feels like: " + Math.round(this.info.main.feels_like)+ "°"
    },
    showHumidity(){
      return "Humadity: " + this.info.main.humidity + "%"
    },
    showWindSpeed(){
      return "Wind Speed: " + this.info.wind.speed +" km/h"
    }

  },
  methods: {
    checkCity() {
      return this.city === "" ? "your city 🌝" : this.capitalizeFirstLetter(this.city);
    },
    capitalizeFirstLetter(string) {
      return string.charAt(0).toUpperCase() + string.slice(1);
    },
    getWeather(){
      if(this.city.trim().length < 3) { 
        this.error = "You should insert a city name with more than two letter"
        return false
      }
      this.error = ""

      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=591a03afdcadbf0cb8784fc560bb36cc`)
      .then(res => this.info = res.data)
      .catch(error => {
      console.error(error);
      this.error = "There is no such a city, please check the spelling error!";
    });
    }
  }
}
</script>

<template>
  <div class="wrapper">
    <h1>Meteo App </h1>
    <p>Check weather in {{ checkCity() }}</p>
    <input type="text" v-model="city" placeholder="Insert city" @keydown.enter="getWeather()">
    <!-- <button v-show=" city != '' ">Check weather</button> -->
    <button v-if=" city != '' " @click="getWeather()">Check weather</button>
    <button disabled v-else>Insert your city</button>
    <p class="error">{{ error}}</p>

    <div v-if="info !== null">
      <p>{{ showTemperature}}</p>
      <p>{{ showFeelsLike}}</p>
      <p>{{ showHumidity}}</p>
      <p>{{ showWindSpeed}}</p>
    </div>
  </div>
</template>



<style scoped>
.error {
  color: red;
}
.wrapper {
  width: 700px;
  height: 500px;
  background: #1f0f24;
  border-radius: 50px;
  padding: 20px;
  text-align: center;
  color: #fff;
}

.wrapper h1 {
margin-top: 50px;
}

.wrapper p {
  margin-top: 20px;
}

.wrapper input {
  margin-top: 30px;
  background-color: transparent;
  border: 0;
  border-bottom: 2px solid #110813;
  color: #fcfcfc;
  padding: 5px 8 px;
  font-size: 14px;
  outline: none;
}

.wrapper input:focus {
  border-bottom-color: #6e2d7d;
}

.wrapper button:disabled{
cursor: not-allowed;
background: #e3bd4b6e;
}

.wrapper button {
  background: #e3bc4b;
  color: #fff;
  border-radius: 10px;
  border: 2px solid #b99935;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease;
}

.wrapper button:hover {
  transform: scale(1.1) translateY(-5px);
}
</style>
