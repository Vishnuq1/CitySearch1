<template>
  <div class="location-detector">
    <div class="location-container">
      <button @click="toggleDropdown" class="location-btn" :disabled="isLoading">
        <span v-if="isLoading">Detecting...</span>
        <span v-else-if="currentLocation">{{ currentLocation }}</span>
        <span v-else>
          <i class="location-icon">📍</i> Detect Location
        </span>
      </button>

      <!-- Dropdown for cities -->
      <div v-if="showDropdown" class="dropdown">
        <div class="dropdown-grid">
          <div 
            v-for="city in cities" 
            :key="city" 
            class="city-item"
            @click="selectCity(city)"
          >
            {{ city }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'LocationDetector',
  props: {
    cities: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      currentLocation: '',
      isLoading: false,
      error: null,
      showDropdown: false
    };
  },
  methods: {
    toggleDropdown() {
      this.showDropdown = !this.showDropdown;
    },
    selectCity(city) {
      this.currentLocation = city;
      this.showDropdown = false;
      this.$emit('location-detected', { city });
    },
    detectLocation() {
      this.isLoading = true;
      
      if (!navigator.geolocation) {
        this.error = "Geolocation is not supported by your browser";
        this.isLoading = false;
        alert(this.error);
        return;
      }
      
      navigator.geolocation.getCurrentPosition(
        this.handleLocationSuccess,
        this.handleLocationError,
        { timeout: 10000, enableHighAccuracy: true }
      );
    },
    handleLocationSuccess(position) {
      const latitude = position.coords.latitude;
      const longitude = position.coords.longitude;
      
      // Reverse geocoding to get city name
      this.getCityFromCoordinates(latitude, longitude);
    },
    handleLocationError(error) {
      this.isLoading = false;
      switch(error.code) {
        case error.PERMISSION_DENIED:
          this.error = "User denied the request for geolocation";
          break;
        case error.POSITION_UNAVAILABLE:
          this.error = "Location information is unavailable";
          break;
        case error.TIMEOUT:
          this.error = "The request to get user location timed out";
          break;
        default:
          this.error = "An unknown error occurred";
          break;
      }
      alert(this.error);
    },
    getCityFromCoordinates(latitude, longitude) {
      // Using a free reverse geocoding API (OpenStreetMap Nominatim)
      fetch(`https://nominatim.openstreetmap.org/reverse?format=json&lat=${latitude}&lon=${longitude}&zoom=10`)
        .then(response => response.json())
        .then(data => {
          // Extract city name from the response
          let city = data.address.city || 
                     data.address.town || 
                     data.address.village || 
                     data.address.county ||
                     "Unknown location";
          
          this.currentLocation = city;
          this.isLoading = false;
          
          // Emit event to parent components with location info
          this.$emit('location-detected', {
            city: city,
            coords: { latitude, longitude },
            fullAddress: data.address
          });
        })
        .catch(error => {
          this.isLoading = false;
          this.error = "Failed to get city name";
          console.error("Geocoding error:", error);
          alert(this.error);
        });
    }
  },
  mounted() {
    // Optional: Auto-detect location when component is mounted
    // this.detectLocation();
  }
};
</script>

<style scoped>
.location-detector {
  display: inline-block;
  position: relative;
}

.location-container {
  position: relative;
}

.location-btn {
  display: flex;
  align-items: center;
  background-color: #f8f9fa;
  border: 1px solid #ddd;
  border-radius: 4px;
  padding: 8px 12px;
  font-size: 14px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.location-btn:hover {
  background-color: #e9ecef;
}

.location-btn:disabled {
  opacity: 0.7;
  cursor: wait;
}

.location-icon {
  margin-right: 5px;
}

.dropdown {
  position: absolute;
  top: 100%;
  left: 0;
  background: white;
  border: 1px solid #ddd;
  border-radius: 4px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  z-index: 1000;
  padding: 10px;
  width: 300px; /* Adjust width as needed */
}

.dropdown-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr); /* 3 columns */
  gap: 10px;
}

.city-item {
  padding: 8px;
  cursor: pointer;
  text-align: center;
  border: 1px solid #eee;
  border-radius: 4px;
  transition: background 0.3s ease;
}

.city-item:hover {
  background: #f0f0f0;
}
</style>