<template>
  <div id="app">
    <h3 id="city-title">{{cityName}}</h3>
    <div id="weather">
     <img class="icon" :src=icon>  {{overcast}}
      <span class="temperature">{{currentTemp}}°</span><br>
      <span id="temp-values">Min {{minTemp}}° <br> Max {{maxTemp}}°</span>
    </div>
    <div id="cityForm">
      Enter City: <input v-model="cityName" type="text" name="fname"><br>
      <button 
        id="submit"
        v-on:click="updateMap"
      >
      Submit
      </button>
      <br>
      <br>
    </div>
    <div id="info">
      <img class="icon" src="images/sunrise.svg"> {{sunrise}}
      <img class="icon" src="images/sunset.svg"> {{sunset}}
      <img class="icon" src="images/humidity.svg"> {{humidity}}
      <img class="icon" src="images/pressure.svg"> {{pressure}}
      <img class="icon" src="images/wind.svg"> {{wind}}
    </div>
  </div>
</template>

<script>
/* eslint-disable no-console */
import axios from 'axios'

export default {
  name: 'app',
  data: function(){
    return{
        currentTemp: '',
        minTemp: '',
        maxTemp:'',
        sunrise: '',
        sunset: '',
        pressure: '',
        humidity: '',
        wind: '',
        overcast: '',
        icon: '',
        cityName: '',
    };
  },
  methods:{
    updateMap : function(){
      axios
      .get("https://api.openweathermap.org/data/2.5/weather?q="+this.cityName+"&?units=metric&APPID=ceaabd1441aec5eb12417e86fc7f7b4f")
      .then(response => {
        this.currentTemp = response.data.main.temp;
          this.minTemp = response.data.main.temp_min;
          this.maxTemp = response.data.main.temp_max;
          this.pressure = response.data.main.pressure;
          this.humidity = response.data.main.humidity + '%';
          this.wind = response.data.wind.speed + 'm/s';
          this.overcast = response.data.weather[0].description;
          this.icon = "images/" + response.data.weather[0].icon.slice(0, 2) + ".svg";
          this.sunrise = new Date(response.data.sys.sunrise*1000).toLocaleTimeString("en-GB").slice(0,4);
          this.sunset = new Date(response.data.sys.sunset*1000).toLocaleTimeString("en-GB").slice(0,4);
          this.cityName = response.data.name;
      })
      .catch(error => {
        console.log(error);
     });
    }
  },
  created: function(){
      axios
      .get("http://api.openweathermap.org/data/2.5/weather?q=Guelph&?units=metric&APPID=ceaabd1441aec5eb12417e86fc7f7b4f")
      .then(response => {
        this.currentTemp = response.data.main.temp;
          this.minTemp = response.data.main.temp_min;
          this.maxTemp = response.data.main.gtemp_max;
          this.pressure = response.data.main.pressure;
          this.humidity = response.data.main.humidity + '%';
          this.wind = response.data.wind.speed + 'm/s';
          this.overcast = response.data.weather[0].description;
          this.icon = "images/" + response.data.weather[0].icon.slice(0, 2) + ".svg";
          this.sunrise = new Date(response.data.sys.sunrise*1000).toLocaleTimeString("en-GB").slice(0,4);
          this.sunset = new Date(response.data.sys.sunset*1000).toLocaleTimeString("en-GB").slice(0,4);
          this.cityName = response.data.name;
      })
      .catch(error => {
        console.log(error);
     });
    }
  }

</script>

<style>
#app {
    background-color: pink ;
    width:    520px;
    height:   350px;
    position: absolute;
    top:     35%;
    left:     35%;
  }
#weather {
  padding: 15px;
  vertical-align: middle;
}
#cityForm{
 text-align: center;
}
#city-title{
    text-align: center;
}
.temperature {
    font-family: 'Vast Shadow', cursive;
    font-size: 40px;
    vertical-align: top;
    position: absolute;
    left: 80px;
  }
 #temp-values {
    text-align: right;
    text-justify: distribute;
    display: block;
    position: relative;
    top: -60px;
  }
  #info {
    padding-left: 20px;
    position: relative;
    top: -20px;
  }
   .icon {
    position: inherit;
    top: 2px;
    padding-left: 8px;
  }
</style>
