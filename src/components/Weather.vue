<template>
  <div class="weather">
    <div class="location">{{location}}</div>
    <div class="date">{{date}}</div>
    <div class="description">{{description}}</div>
    <div class="current">
      <div class="visual">
        <div class="icon sunny" alt="sunny"></div>
        <div class="temp">{{temp}}<div class="scale">º</div></div>

      </div>
      <div class="text">
        <div class="precipitation">Precipitation: {{precipitation}}</div>
        <div class="humidity">Humidity: {{humidity}}</div>
        <div class="wind">Wind: {{wind}}</div>
        <div class="pollen">Pollen: {{pollen}}</div>
      </div>
    </div>
    <div class="forecast">
      <div class="forecast-day" v-for="forecast in forecasts">
        <div class="date">{{forecast.date}}</div>
        <div v-bind:class="'icon ' + forecast.icon"></div>
        <div class="high-temp">{{forecast.hightemp}}</div>
        <div class="low-temp">{{forecast.lowtemp}}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'weather',
  data () {
    return {
      location: 'Tortosa, TRT',
      date: 'Friday, August 11th',
      description: 'Summer time',
      temp: 45,
      precipitation: '10%',
      humidity: '9%',
      wind: '4mph NW',
      pollen: 76,
      forecasts: []
    }
  },
  methods: {
    fetchWeather: function () {
      this.$http.get('http://localhost:3000/weather').then((response) => {
        console.log(response.data)
        this.connecting = false
        this.forecasts = response.data
      }, (response) => {
        console.log(response.data)
        this.connecting = false
        this.showConnectionError()
        this.authorized = false
      })
    },
    showConnectionError: function () {
      // TOAST : toastjs TODO
    }
  },
  created: function () {
    this.fetchWeather()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.weather{
  width: 100%;
  padding: 20px;
  box-shadow: 0 100px 100px rgba(0,0,0,0.2);
}

@media (min-width:700px){
  .weather {
    width: 700px;
  }
}

.location {
  font-size: 3em;
  color: black;
}

.date {
  font-size: 1.5em;
}

.description {
  font-size: 1.1em;
}

.current {
  overflow:auto;
  width: 100%;
}

.visual {
  float: left;
  width: 50%;
}

.text {
  float: right;
  width: 50%;
}

.forecast-day{
  display: inline-block;
  width: 14.285%;
}

.visual .icon {
  width:64px;
  height: 64px;
}

.forecast-day .icon {
  width: 64px;
  height: 64px;
}

.forecast-day .date {
  color: black;
  font-size: 0.5em;
}

.icon {
  background-repeat: no-repeat;
  background-size: contain;
  margin-left: auto;
  margin-right: auto;
}

.icon.cloudy {
  background-image: url("../assets/cloudy.png");
}

.icon.sunny {
  background-image: url("../assets/sunny.png");
}

.icon.stormy {
  background-image: url("../assets/thunderstorms.png");
}

.icon.partcloudy {
  background-image: url("../assets/partly_cloudy.png");
}

.icon.rainy {
  background-image: url("../assets/rain_s_cloudy.png");
}

.icon.rain {
  background-image: url("../assets/rain.png");
}

@media (max-width: 650px){
  .location{
    font-size: 5em;
  }

  .forecast-day {
    display: block;
    border-top: 1px solid black;
    width:100%;
  }
  .forecast-day .date {
    width: 50%;
    text-align: left;
  }

  .date, .icon, .high-temp, .low-temp {
    display: inline-block;
  }
}

</style>
