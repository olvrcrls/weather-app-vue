<script setup>
  import { ref } from "vue";
  import axios from 'axios';
  
  const mapboxAPIKey = "pk.eyJ1IjoienhjdmYiLCJhIjoiY2w3bTB6eXUzMWhoNTN2dDgwZjA5NzhlZyJ9.x53t0hqfWa9R84B47WqrQw";
  const searchQuery = ref("");
  const queryTimeout = ref(null);
  const mapboxSearchResults = ref(null);
  
  const getSearchResults = () => {
    queryTimeout.value = setTimeout(async () => {
      clearTimeout(queryTimeout.value);
      if (searchQuery.value !== "") {
        const result = await axios.get(`https://api.mapbox.com/geocoding/v5/mapbox.places/${searchQuery}.json?access_token=${mapboxAPIKey}&types=place`);
        mapboxSearchResults.value = result.data.features;
        return;
      }
      mapboxSearchResults.value = null;
    }, 300);
  };
</script>

<template>
  <main class="container text-white">
    <div class="relative pt-4 mb-8">
      <input type="text" 
      v-model="searchQuery"
      @input="getSearchResults()"
      placeholder="Search for a city or state" 
      class="w-full px-1 py-2 bg-transparent border-b focus:border-weather-secondary focus:outline-none focus:shadow-[0px_1px_0_0_#004E71]"
      >

      <ul v-if="mapboxSearchResults" class="absolute w-full px-1 py-2 text-white shadow-md bg-weather-secondary top-[66]">
        <li v-for="searchResult in mapboxSearchResults" :key="searchResult.id" class="py-2 cursor">
          {{ searchResult.place_name }}
        </li>
      </ul>
    </div>
  </main>
</template>
