<template>
<div class="weather">
  <div class="city">
    <p>{{this.city}}, {{data.country}}</p>
  </div>
  <div class="temp">
    <img :src="my_src">
    <p>{{data.temp}}&deg;</p>
  </div>
  <div class="text">
    <p>Feels like {{data.midleTemp}}&deg;C. {{data.stateTemp}}. {{data.stateTemp2}}</p>
  </div>
  <div>
    <div class="items">
      <div class="item">
        <img src="../assets/1.svg">
        <p>{{data.sSE}}m/s SSE</p>
      </div>
      <div class="item">
        <i class="fi fi-bs-comment-pen"></i>
        <p>{{data.hPa}}hPa</p>
      </div>
    </div>
    <div class="items">
      <div class="item">
        <p>Humidity {{data.Humidity}}%</p>
      </div>
      <div class="item">
        <p>Dew point {{data.Dew_point}}&deg;C</p>
      </div>
    </div>
    <p class="visibility">Visibility: {{data.Visibility}}km</p>
  </div>
</div>
</template>

<script>
import axios from "axios";

export default {
  name: 'WeatherComponent',
  props:{
    city:{
      type: String
    },
    src:{
      type: String
    }
  },
  data(){
    return{
      data:{
        city:'',
        weather: null,
        current_day:'',
        img: '',
        temp: 0,
        country: '',
        midleTemp: 0,
        stateTemp: '',
        stateTemp2: '',
        sSE: 0,
        hPa: 0,
        Humidity: '',
        Dew_point: 0,
        Visibility: 0,
        state_weather:false
      },
      my_src: ''
    }
  },
  mounted: async function(){
    this.getApi()

  },
  methods:{
    async getApi(){
      this.data.city = this.city
      if(this.data.city===''){
        this.data.city = 'London'
      }
      const getWeather = await axios.get(`${this.src}&units=metric&q=${this.data.city}`)
      console.log(getWeather);
      this.data.weather = getWeather.data
      this.data.city = ''
      console.log(this.my_src)
      this.data.img = getWeather.data.weather[0].icon;
      this.data.temp = this.data.weather.main.temp;
      this.data.midleTemp = this.data.weather.main.feels_like;
      this.data.stateTemp2 = getWeather.data.weather[0].main;
      this.data.stateTemp = getWeather.data.weather[0].description;
      this.data.country = this.data.weather.sys.country;
      this.data.sSE = this.data.weather.wind.speed;
      this.data.hPa = this.data.weather.main.pressure;
      this.data.Visibility = this.data.weather.visibility / 1000;
      this.data.Humidity = this.data.weather.main.humidity;
      this.my_src = `https://openweathermap.org/img/wn/${this.data.img}@2x.png`;
      console.log(this.data.img)
      if(this.data.weather.main.temp > 16){
        this.state_weather = true
      }else{
        this.state_weather = false
      }
    }
  }
}
</script>

<style scoped>
@import url('https://cdn-uicons.flaticon.com/uicons-solid-straight/css/uicons-solid-straight.css');
@import url('https://cdn-uicons.flaticon.com/uicons-bold-straight/css/uicons-bold-straight.css');
p, i{
  margin: 0;
}
.weather{
  width: 100%;
  padding: 10px;
  margin: 40px 0;
}
.temp{
  margin: 10px 0;
  display: flex;
  justify-content: center;
}
.temp img{
  width: 24%;
  height: auto;
}
.temp p{
  margin: 16px 0;
  font-size: 28px;
  font-weight: 700;
  margin-left: 10px;
}
.items{
  display: flex;
  justify-content: space-between;
}
.items .item{
  margin: 8px 0;
  display: flex;
}
.items .item i{
  margin-right: 8px;
  font-size: 14px;
}
.items .item img{
  width: 14px;
  margin-right: 8px;
}
.visibility{
  margin: 8px 0;
}
</style>