<template>
    <div class="text-white-mb-8">
        <div class="places-input text-gray-800">
          <input type="text" class="w-full">
        </div>

        <div class="current-weather">
          <div class="grid grid-cols-1">
            <p class="font-extrabold text-2xl text-center">{{ location.name }}, {{ location.country}}</p>
          </div>
          <div class="grid grid-cols-2">
            <div>
              <p class="font-medium text-xl">{{ currentWeather.actual_temp }}°C</p>
            </div>
            <div>
              <img :src="'http://openweathermap.org/img/wn/' + currentWeather.icon + '@2x.png'" alt="">
            </div>
          </div>
          <div class="grid grid-cols-2">
            <div>
              <p class="text-sm">
                <span class="font-extrabold">Max:</span> {{ currentWeather.temp_min }}°C
              </p>
              <p class="text-sm">
                <span class="font-extrabold">Max:</span>  {{ currentWeather.temp_max }}°C
              </p>
            </div>
            <div>
              <p>Feels Like {{ currentWeather.feels_like }} °C</p>
              <p>{{ currentWeather.description }}</p>
            </div>
          </div>
          <div class="grid grid-cols-3 p-6">
            <div>
              <p class="font-medium">Pressure: </p>
              <p>{{ currentWeather.pressure }} hPa</p>
            </div>
            <div>
              <p class="font-medium">Humidity: </p>
              <p>{{ currentWeather.humidity }}%</p>
            </div>
            <div>
              <p class="font-medium">Wind: </p>
              <p>{{ currentWeather.wind_speed }}mps</p>
            </div>
          </div>
        </div>
    </div>
</template>

<script>
  export default {
    mounted() {
      this.fetchData()
    },
    data(){
      return {
        currentWeather: {
          actual_temp: '',
          feels_like: '',
          description: '',
          icon:'',
          temp_min: '',
          temp_max: '',
          pressure: '',
          humidity: '',
          wind_speed: ''
        },
        location: {
          name: 'london',
          country: 'GB',
        }
      }
    },
    methods: {
      fetchData() {
        fetch(`/api/weather?city=${this.location.name}`)
          .then(response => response.json())
          .then(data => {
            console.log(data)
            this.currentWeather.actual_temp = Math.round(data.main.temp)
            this.currentWeather.feels_like = Math.round(data.main.feels_like)
            this.currentWeather.description = data.weather[0].description
            this.currentWeather.icon = data.weather[0].icon
            this.currentWeather.temp_min = Math.round(data.main.temp_min)
            this.currentWeather.temp_max = Math.round(data.main.temp_max)
            this.currentWeather.pressure = data.main.pressure
            this.currentWeather.humidity = data.main.humidity
            this.currentWeather.wind_speed = data.wind.speed
          })
      }
    }
  }

</script>
