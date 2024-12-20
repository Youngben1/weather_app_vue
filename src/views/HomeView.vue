<template>
  <main class="container text-white">
    <div class="pt-4 mb-8 relative">
      <input type="text" v-model="searchQuery" @input="getSearchResults" placeholder="Search for a city or angwa" class="py-2 font-bold px-1 w-full bg-transparent border-b focus:border-weather-secondary focus:outline-none focus:shadow-[0px_1px_0_0_#004E71]" />
    </div>
  </main>
</template>

<script setup>
import {ref} from "vue" 
import axios from "axios"


  const mapboxAPIKey = "VUE_APP_ENV_VARIABLE"
  const searchQuery = ref("");
  const queryTimeout = ref(null);
  const mapboxSearchResults = ref(null);

  const getSearchResults = () => {

    clearTimeout(queryTimeout.value);
    queryTimeout.value = setTimeout(async () => {
      if(searchQuery !== "") {
      const result = await axios.get(`https://api.mapbox.com/geocoding/v5/mapbox.places/${searchQuery.value}.json?access_token=${mapboxAPIKey}&types=place`);
        mapboxSearchResults.value = result.data.features;
        console.log(mapboxSearchResults.value)
        return;
      }
      mapboxSearchResults.value = null;
    }, 300);
  }
</script>
