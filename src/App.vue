<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <div class="row">
      <div class="col-md-4">
      </div>
      <div class="col-md-4 weather-app">
        <form @submit.prevent="refresh()">
        <input type="text" v-model="search" class="form-control mt-3" autofocus placeholder="Search Location . . .">
        </form>
          <div class="weather"></div>
        <h2 class="font-italic">{{ weather.location }} , {{ weather.country }}</h2>
        <h2 class="mt-3 font-weight-bold celcius">{{ Math.round(weather.temp) }}&deg; C</h2>
        <h2 class="mt-3">{{ weather.name }}</h2>
        <img src="" alt="">
      </div>
      <div class="col-md-4"></div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

  export default {
    name: 'app',
    data () {
      return {
        api_keys : 'c06433fddfc3f9ff36183163caa3179a',
        search : '',
        weather : {
          country : '',
          location : '',
          temp : '',
          name : ''
        }
      }
    },
    created() {
      this.load()
    },
    methods : {
      load(){
        axios.get('http://api.openweathermap.org/data/2.5/weather?q=indonesia&units=metric&APPID=c06433fddfc3f9ff36183163caa3179a').then(res => {
          this.weather.location = res.data.name
          this.weather.country = res.data.sys.country
          this.weather.temp = res.data.main.temp  
          this.weather.name = res.data.weather[0].main
        })
      },
      refresh() {
        axios.get('http://api.openweathermap.org/data/2.5/weather?q='+this.search+'&units=metric&APPID=c06433fddfc3f9ff36183163caa3179a').then(res => {
          this.weather.location = res.data.name
          this.weather.country = res.data.sys.country
          this.weather.temp = res.data.main.temp  
          this.weather.name = res.data.weather[0].main
          this.search = ''
        })
      }
    }
  }
</script>

<style>
  @import url('https://fonts.googleapis.com/css?family=Acme&display=swap');

  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
  }

  .weather-app {
    font-family: 'Acme', sans-serif;
    display: block;
  }

  .weather {
    margin-top: 40px; 
  }

  .celcius {
    font-size: 40px;
  }
</style>