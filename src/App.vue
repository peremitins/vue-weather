<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input 
          type="text" 
          class="search-bar" 
          placeholder="Search..."
          @keyup.enter="fetchWeather"
          v-model="query"
        >
      </div>
      
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <img class="weather-img" :src="icon" alt="icons">
          <div class="weather">{{ weather.weather[0].main}}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      api_key: 'ab96b8af98e69f95d59bf51a0d5968e7',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: 'краснодар',
      weather: {},
      icon: ''
    }
  },
  created() {
    this.fetchWeather();
  },
  methods: {
    fetchWeather() {
      
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&appid=${this.api_key}&lang=ru`)
        .then(response => response.json())
        .then(data => {
          this.weather = data;
          this.icon = `http://openweathermap.org/img/w/${data.weather[0].icon}.png`
          this.query = '';
        })
        
    },
    dateBuilder() {
      let d = new Date();
      let months = ['Январь','Февраль','Март','Апрель','Май','Июнь','Июль','Август','Сентябрь','Октябрь','Ноябрь','Декабрь'];
      let days = ['Воскресенье','Понедельник','Вторник','Среда','Четверг','Пятница','Суббота'];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      
      return `${day} ${date} ${month} ${year}`;
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: 'montserrat', san-serif;
}

#app {
  background-image: url(./assets/cold-bg.jpg);
  background-size: cover;
  background-position: bottom;
  transition: .4s;
}
#app.warm {
  background-image: url(./assets/warm-bg.jpg);
}
main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0,0,0,.25), rgba(0,0,0,.75));
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
  box-sizing: 0px 0px 8px rgba(0,0,0,.25);
  background-color: rgba(255,255,255,.5);
  border-radius: 0px 16px 0px 16px;
  transition: .4s;
}
.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0,0,0,.25);
  background-color: rgba(255,255,255,.75);
  border-radius: 16px 0px 16px 0px;
}
.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0,0,0,.25);
}
.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  text-align: center;
  font-style: italic;
}
.weather-box {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
}
.weather-img {
  width: 50px;
  max-width: 100%;
  object-fit: cover;
}
.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;
  
  text-shadow: 3px 6px rgba(0,0,0,.25);
  background-color: rgba(255,255,255,.25);
  border-radius: 16px;
  margin: 30px 0;
  box-shadow: 3px 6px rgba(0,0,0,.25);
}
.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0,0,0,.25);
}

</style>
