<template>
	<main class="container text-white">
		<div class="pt-4 mb-8 relative">
			<input
				type="text"
				v-model="searchQuery"
				@input="getSearchResults"
				placeholder="Search for a city or angwa"
				class="py-2 font-bold px-1 w-full bg-transparent border-b focus:border-weather-secondary focus:outline-none focus:shadow-[0px_1px_0_0_#004E71]"
			/>
			<ul
				class="absolute text-white w-full shadow-md px-1 py-2 bg-weather-secondary top-[66px]"
				v-if="mapboxSearchResults"
			>
				<p v-if="searchError">No vex, e no de work abeg</p>
				<p v-if="!searchError && mapboxSearchResults.length === 0">
					The rubbish wey you de type no de
				</p>
				<template v-else>
					<li
						v-for="searchResult in mapboxSearchResults"
						:key="searchResult.id"
						class="py-2 cursor-pointer"
						@click="previewCity(searchResult)"
					>
						{{ searchResult.place_name }}
					</li>
				</template>
			</ul>
		</div>
	</main>
</template>

<script setup>
import { ref } from "vue";
import axios from "axios";
import {useRouter} from "vue-router";


const router = useRouter();
const previewCity = () => {
	const [city, state] = searchResult.place_name.split('','')
	router.push({
		name: 'cityView',
		params: { state: state.replaceAll("",""), city: city},
		query: {
		lng: searchResult.geometry.coordinates[1],
		lat: searchResult.geometry.coordinates[0],
		preview: true,
		} 
	});
}

const mapboxAPIKey = "VUE_APP_ENV_VARIABLE";
const searchQuery = ref("");
const queryTimeout = ref(null);
const mapboxSearchResults = ref(null);
const searchError = ref(null);

const getSearchResults = () => {
	clearTimeout(queryTimeout.value);
	queryTimeout.value = setTimeout(async () => {
		if (searchQuery !== "") {
			try {
				const result = await axios.get(
					`https://api.mapbox.com/geocoding/v5/mapbox.places/${searchQuery.value}.json?access_token=${mapboxAPIKey}&types=place`
				);
				mapboxSearchResults.value = result.data.features;
			} catch {
				searchError.value = true;
			}
			return;
		}
		mapboxSearchResults.value = null;
	}, 300);
};
</script>
