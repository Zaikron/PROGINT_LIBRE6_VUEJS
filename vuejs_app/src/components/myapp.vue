<template>
    <div class="w-full h-screen flex justify-center items-center bg-orange-50">

        <div class="grid grid-cols-1 lg:w-3/5 md:w-3/4 sm:w-3/4 h-3/5 border-2 shadow-xl bg-white rounded-md p-8">
            <div class="flex justify-center items-center">
                <p class="text-gray-800 lg:text-3xl md:text-2xl sm:text-xl font-bold px-4">Seleccione una ciudad: </p>
                <select v-model="selectedCity" class="bg-orange-800 rounded-md w-1/3 p-2 text-white font-bold lg:text-lg md:text-md sm:text-md">
                    <option value="Guadalajara">Guadalajara</option>
                    <option value="Zapopan">Zapopan</option>
                    <option value="Tlaquepaque">Tlaquepaque</option>
                    <option value="Tonala">Tonalá</option>
                </select>
            </div>
                
            <div class="flex items-center justify-center">
                <button @click="getWeather" class="bg-red-800 text-white font-bold px-4 py-2 rounded-md lg:text-lg md:text-md sm:text-md">Obtener Clima</button>
            </div>

            <div class="flex justify-center items-center">
                <img src="./weather.png" alt="" class="w-48 h-48 mr-6">
                <div v-if="weatherInfo">
                    <p class="lg:text-3xl md:text-2xl sm:text-xl text-gray-800 inline">Clima en </p> <p class="lg:text-3xl md:text-2xl sm:text-xl text-orange-800 inline">{{ weatherInfo.name }}, {{ weatherInfo.sys.country }}</p><br>
                    <p class="lg:text-2xl md:text-xl sm:text-lg text-gray-800 inline">Temperatura: </p> <p class="lg:text-2xl md:text-xl sm:text-lg text-red-800 inline">{{ convertKelvinToCelsius(weatherInfo.main.temp) }}°C</p><br>
                    <p class="lg:text-2xl md:text-xl sm:text-lg text-gray-800 inline">Estado: </p> <p class="lg:text-2xl md:text-xl sm:text-lg text-red-800 inline">{{ weatherInfo.weather[0].description }}</p><br>
                </div>
                <p v-if="error" class="text-red-500">{{ error }}</p>
            </div>
        </div>

    </div>
</template>
  
<script setup>
    import { ref } from 'vue'
    
    const selectedCity = ref('Guadalajara') // Establece la ciudad predeterminada
    const weatherInfo = ref(null)
    const error = ref('')
    
    const clearWeatherInfo = () => {
        weatherInfo.value = null
        error.value = ''
    }
    
    const getWeather = async () => {
        try {
        const apiKey = 'abee36f6eea7f6071283480f8c593544' 
        const apiUrl = `https://api.openweathermap.org/data/2.5/weather?q=${selectedCity.value}&appid=${apiKey}`
        
        const response = await fetch(apiUrl)
        const data = await response.json()
        
        if (response.ok) {
            weatherInfo.value = data
        } else {
            error.value = `Error al obtener datos del clima: ${data.message}`
        }
        } catch (e) {
        error.value = 'Error de red al obtener datos del clima'
        }
    }
    
    const convertKelvinToCelsius = (kelvin) => {
        return (kelvin - 273.15).toFixed(2);
    }
</script>
  
<style scoped>
</style>
  