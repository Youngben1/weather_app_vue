<template>
	<div class="flex items-centre flex-col flex-1">
    <!-- Banner -->
        <div v-if="route.query.preview" class="text-white text-center p-4 w-full bg-weather-secondary">
            <p>You are currently previewing this city, click the "+" icon to start tracking this City</p>
        </div>
    <!-- Weather Oveview -->
    <div class="flex flex-col items-center py-12 text-white">
        <h1 class="text-4xl mb-2">{{ route.params.city }}</h1>
        <p class="text-sm mb-12">
            {{ new Date(weatherData.currentTime).toLocaleDateString(
                "en-us",
                {
                    weekday: "short",
                    day: "2-digit",
                    month: "long"
                }
            )
            }}
            {{ new Date(weatherData.currentTime).toLocaleDateString(
                "en-us",
                {
                    timeStyle: "short",
                }
            )
            }}
        </p>
    </div>
    </div>
</template>

<script setup>
import axios from "axios";
import { useRoute } from "vue-router";

const Appid = "VUE_APP_ENV_ID"
const route = useRoute();
const getWeatherData = async () => {
	try {
		const weatherData = await axios.get(
			`https://api.openweathermap.org/data/2.5/onecall?lat=${route.query.lat}&lon=${route.query.lng}&exclude={part}&appid=Appid&units=imperial`
		);
		// cal current date & time
		const localOffset = new Date().getTimezoneOffset() * 60000;
		const utc = weatherData.data.current.dt * 1000 + localOffset;
		weatherData.data.currentTime =
			utc + 1000 * weatherData.data.timezone_offset;

		// cal hourly weather offset
		weatherData.data.hourly.forEach((hour) => {
			const utc = hour.dt * 1000 + localOffset;
			hour.currentTime = utc + 1000 * weatherData.data.timezone_offset;
		});

		return weatherData;
	} catch (err) {
		console.log(err);
	}
};
const weatherData = await getWeatherData();
</script>
