<script setup>
import axios from 'axios';
</script>

<template>
  <div class="container">
    <div class="form-group">
      <input class="form-control" type="text" v-model="searchTerm" placeholder="Search locations..." @input="searchLocation">
      <div v-if="results.length" class="dropdown-menu">
        <a 
          href="#"
          v-for="result in results" :key="result.place_id"
          class="dropdown-item"
          @click.prevent="selectLocation(result)"
        >
          {{ result.display_name }}
        </a>
      </div>
    </div>
    <div class="form-row">
      <div class="form-group col-md-6">
        <label for="latitude">Latitude</label>
        <input class="form-control" id="latitude" v-model="latitude" readonly>
      </div>
      <div class="form-group col-md-6">
        <label for="longitude">Longitude</label>
        <input class="form-control" id="longitude" v-model="longitude" readonly>
      </div>
    </div>
  </div>
  
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      searchTerm: '',
      results: [],
      selectedLocation: null,
      latitude: '',
      longitude: '',
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
      this.selectedLocation = location;
      this.latitude = location.lat;
      this.longitude = location.lon;
    },
  },
};
</script>

<style>
/* Optional CSS styles */
.dropdown-menu {
  position: relative;
  display: block;
  margin: 0;
  border: 1px solid #ddd;
  border-top: none;
  width: 100%;
  max-height: 200px;
  overflow-y: auto;
  background-color: #fff;
}
.dropdown-item {
  padding: 0.5rem 1rem;
  font-size: 1rem;
  color: #373a3c; 
  text-align: inherit;
  white-space: nowrap;
  background-color: transparent;
  border: none;
  border-radius: 0;
  cursor: pointer;
}
.dropdown-item:hover,
.dropdown-item:focus {
  color: #2a9fd6;
  text-decoration: none;
  background-color: #f5f5f5;
}
</style>
