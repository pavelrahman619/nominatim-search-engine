<script setup>
import axios from 'axios';
</script>

<template>
  <div>
    <input v-model="searchTerm" placeholder="Search locations..." @input="searchLocation">
    <ul v-if="results.length">
      <li v-for="result in results" :key="result.place_id" @click="selectLocation(result)">
        {{ result.display_name }}
      </li>
    </ul>
    <input v-model="selectedLocation.latitude" placeholder="Latitude" readonly>
    <input v-model="selectedLocation.longitude" placeholder="Longitude" readonly>
  </div>
  
</template>

<script>
export default {

  // methods: {
  //   async geocodeAddress(address) {
  //     const response = await axios.get(`https://nominatim.openstreetmap.org/search?q=${encodeURIComponent(address)}&format=json`);
  //     if (response.status !== 200) {
  //       throw new Error(`Nominatim API request failed with status: ${response.status}`);
  //     }
  //     if (response.data.length === 0) {
  //       throw new Error(`No geocode results found for address: ${address}`);
  //     }
  //     const location = response.data[0];
  //     return {
  //       latitude: location.lat,
  //       longitude: location.lon
  //     };
  //   }
  // },

  // async mounted() {
  //   const result = await this.geocodeAddress('1600 Amphitheatre Parkway, Mountain View, CA');
  //   console.log(result.latitude, result.longitude);
  // }



  data() {
    return {
      searchTerm: '',
      results: [],
      selectedLocation: {
        latitude: '',
        longitude: '',
      },
    };
  },
  methods: {
    searchLocation() {
      axios.get(`https://nominatim.openstreetmap.org/search?format=json&q=${this.searchTerm}`)
        .then(response => {
          this.results = response.data;
        })
        .catch(error => {
          console.error(error);
        });
    },
    selectLocation(location) {
      this.selectedLocation.latitude = location.lat;
      this.selectedLocation.longitude = location.lon;
    },
  },
};
</script>
