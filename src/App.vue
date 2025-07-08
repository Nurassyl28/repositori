<script>
import axios from 'axios'
export default {
  data() {
    return {
      city: '',
      error: '',
      info: null,
    }
  },
  computed: {
    cityName() {
      return "<" + this.city + ">"
    },
    showTemp() {
      return  'Weather temp: ' + this.info.main.temp
    },
    showFiling() {
      return  'Feels like: ' + this.info.main.feels_like 
    },
    showTempMin() {
      return 'Min temp: ' + this.info.main.temp_min 
    },
    showTempMax() {
      return  'Max temp: ' + this.info.main.temp_max 
    }
  },
  methods: {
    getWeather() {
      if (this.city.trim().length < 2) {
        this.error = "Please enter more than one symbol"
        return
      }
      this.error = ''
      this.info = null

      axios
        .get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=b84005f382f7ecd6883a172d3f6caf5e`)
        .then(res => (this.info = res.data))
        .catch(err => {
          this.error = "City not found"
        })
    }
  }
}
</script>

<template>
  <div class="wrapper">
    <h1>Application for Weather</h1>
    <p>Know the weather in {{ city === '' ? 'your city' : cityName }}</p>
    
    <input type="text" v-model="city" placeholder="Write your city" />

    <button @click="getWeather">
      Get weather
    </button>

    <p class="error">{{ error }}</p>

    <div v-if="info != null">
      <p>{{ showTemp }}</p>
      <p>{{ showFiling }}</p>
      <p>{{ showTempMin }}</p>
      <p>{{ showTempMax }}</p>
    </div>
  </div>
</template>

<style scoped>
.wrapper {
  width: 900px;
  height: 500px;
  border-radius: 50px;
  padding: 20px;
  background: radial-gradient(circle, rgba(238, 174, 202, 1) 0%, rgba(148, 187, 233, 1) 100%);
  text-align: center;
  color: white;
}

h1 {
  margin-top: 50px;
}

p {
  margin-top: 20px;
}

input {
  margin-top: 30px;
  background-color: transparent;
  border: 0;
  border-bottom: 2px solid #EEAECA;
  outline: none;
  padding: 5px 8px;
  font-size: 14px;
  color: blue;
}

input:focus {
  border-bottom-color: #E100FF;
}

button {
  background: #e3bc4b;
  color: #fff;
  border-radius: 10px;
  border: 2px solid #b99935; 
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease;
}

button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

button:hover:not(:disabled) {
  transform: scale(1.1) translateY(-5px);
}

.error {
  color: red;
}
</style>