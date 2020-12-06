<template>
    <div class="text-white">
        <div>
          <p class="text-4xl text-center text-green-600 mb-5">Today's weather</p>
        </div>
        <div class="places-input text-gray-800">
          <input type="search" id="address" class="form-control rounded w-full" placeholder="Type the location..." />
          <p class="text-white">Selected: <strong id="address-value">none</strong></p>
        </div>

        <div class="border border-2 border-green-600 mt-5 rounded">
          <div class="grid grid-cols-1 bg-green-600 py-3 border-b border-green-600">
            <p class="font-extrabold text-xl text-center">{{ location.city }}, {{ location.country}}</p>
          </div>
          <div class="grid grid-cols-2">
            <div class="flex items-center">
              <p class="border-custom text-center font-medium text-4xl w-full">{{ currentWeather.actual_temp }} °C</p>
            </div>
            <div class="mx-auto">
              <img :src="'http://openweathermap.org/img/wn/' + currentWeather.icon + '@2x.png'" alt="">
            </div>
          </div>
          <div class="grid grid-cols-2 text-center">
            <div>
              <p class="text-sm">
                <span class="font-bold">Max:</span> {{ currentWeather.temp_min }} °C
              </p>
              <p class="text-sm">
                <span class="font-bold">Max:</span>  {{ currentWeather.temp_max }} °C
              </p>
            </div>
            <div class="text-center">
              <p><span class="font-semibold">Feels Like: </span> {{ currentWeather.feels_like }} °C</p>
              <p class="capitalize">{{ currentWeather.description }}</p>
            </div>
          </div>
          <div class="grid grid-cols-3 p-6 text-center mt-6 border-t">
            <div class="m-3">
              <p class="font-medium">Pressure: </p>
              <p>{{ currentWeather.pressure }} hPa</p>
            </div>
            <div class="m-3">
              <p class="font-medium">Humidity: </p>
              <p>{{ currentWeather.humidity }}%</p>
            </div>
            <div class="m-3">
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

      var placesAutocomplete = places({
          appId: 'plUAO09WY4WH',
          apiKey: 'ddac726479828649356cb2312d526307',
          container: document.querySelector('#address')
      }).configure({
        type: 'city',
        aroundLatLngViaIP: false,
      });

      var $address = document.querySelector('#address-value')
        placesAutocomplete.on('change', (e) => {
        $address.textContent = e.suggestion.name + ', ' + e.suggestion.country

        this.location.city = `${e.suggestion.name}`
        this.location.country = `${e.suggestion.country}`
      });

      placesAutocomplete.on('clear', function() {
        $address.textContent = 'none';

      });
    },
    watch: {
      location: {
        handler(newValue, oldvalue){
          this.fetchData()
        },
        deep: true
      }
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
          city: 'London',
          country: 'GB',
        }
      }
    },
    methods: {
      fetchData() {
        fetch(`/api/weather?city=${this.location.city}`)
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

<style scoped>
  /* * {
    border: 1px solid red;
  }

  .border-custom {
    border: 1px solid green;
    max-width: 100%;
  } */
</style>
