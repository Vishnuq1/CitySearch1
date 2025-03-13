<!-- <template>
  <div class="location-detector">
    <div class="location-container">
      <button @click="toggleDropdown" class="location-btn" :disabled="isLoading">
        <span v-if="isLoading">Detecting...</span>
        <span v-else-if="currentLocation">{{ currentLocation }}</span>
        <span v-else>
          <i class="location-icon">📍</i> Detect Location
        </span>
      </button>

      drop down
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
  width: 300px; 
}

.dropdown-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr); 
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
</style> -->
<!-- 









<template>
  <div class="location-detector">
    <div class="location-container">
      
      <button @click="handleButtonClick" class="location-btn" :disabled="isLoading">
        <span v-if="isLoading">Detecting...</span>
        <span v-else-if="currentLocation">{{ currentLocation }}</span>
        <span v-else>
          <i class="location-icon">📍</i> Detect Location
        </span>
      </button>

     
      <div v-if="showDropdown" class="dropdown">
        
        <div v-if="isFetchingCities" class="loading">
          Loading cities...
        </div>

      
        <div v-else-if="fetchCitiesError" class="error">
          Error: {{ fetchCitiesError }}
        </div>

       
        <div v-else>
          <div v-for="city in cities" :key="city.name" class="city-item">
            <div class="city-header" @click="toggleCity(city.name)">
              {{ city.name }}
              <span class="toggle-icon">{{ expandedCity === city.name ? '▲' : '▼' }}</span>
            </div>
            <div v-if="expandedCity === city.name" class="properties-grid">
              <div 
                v-for="property in city.properties" 
                :key="property.id" 
                class="property-item"
                @click="selectProperty(property)"
              >
                <img :src="property.image" :alt="property.title" class="property-image">
                <div class="property-info">
                  <h4>{{ property.title }}</h4>
                  <p class="property-price">${{ property.price.toLocaleString() }}</p>
                  <div class="property-details">
                    <span>{{ property.bedrooms }} beds</span>
                    <span>{{ property.bathrooms }} baths</span>
                    <span>{{ property.sqft.toLocaleString() }} sqft</span>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'LocationDetector',
  data() {
    return {
      currentLocation: '',
      isLoading: false,
      error: null,
      showDropdown: false,
      expandedCity: null, // Track which city's properties are expanded
      cities: [], // Initialize cities as an empty array
      isFetchingCities: false, // Track if cities are being fetched
      fetchCitiesError: null // Track errors while fetching cities
    };
  },
  methods: {
    // Fetch cities from the backend
    async fetchCities() {
      this.isFetchingCities = true;
      this.fetchCitiesError = null;

      try {
        const response = await fetch('https://your-backend-api.com/cities'); // Replace with your API endpoint
        if (!response.ok) {
          throw new Error('Failed to fetch cities');
        }
        const data = await response.json();
        this.cities = data; // Update the cities array with data from the backend
      } catch (error) {
        this.fetchCitiesError = error.message;
        console.error('Error fetching cities:', error);
      } finally {
        this.isFetchingCities = false;
      }
    },
    handleButtonClick() {
      // If dropdown is open, close it
      if (this.showDropdown) {
        this.showDropdown = false;
        return;
      }

      // If location is already detected, open the dropdown
      if (this.currentLocation) {
        this.showDropdown = true;
        return;
      }

      // Otherwise, detect location
      this.detectLocation();
    },
    toggleCity(cityName) {
      // Toggle the expanded city
      this.expandedCity = this.expandedCity === cityName ? null : cityName;
    },
    selectProperty(property) {
      this.currentLocation = property.city; // Set the selected city
      this.showDropdown = false;
      this.$emit('location-detected', { city: property.city });
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

          console.error("Full error:", error);
          this.fetchCitiesError = error.message;

          alert(this.error);
        });
    }
  },
  mounted() {
    // Fetch cities when the component is mounted
    this.fetchCities();
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
  width: 400px; 
  max-height: 400px;
  overflow-y: auto; 
}

.city-item {
  margin-bottom: 10px;
}

.city-header {
  font-weight: bold;
  cursor: pointer;
  padding: 8px;
  background-color: #f0f0f0;
  border-radius: 4px;
  transition: background 0.3s ease;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.city-header:hover {
  background-color: #e0e0e0;
}

.toggle-icon {
  font-size: 12px;
}

.properties-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 10px;
  margin-top: 10px;
}

.property-item {
  border: 1px solid #eee;
  border-radius: 4px;
  overflow: hidden;
  transition: transform 0.3s ease;
  cursor: pointer;
}

.property-item:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 20px rgba(0,0,0,0.1);
}

.property-image {
  width: 100%;
  height: 150px;
  object-fit: cover;
}

.property-info {
  padding: 10px;
}

.property-info h4 {
  margin: 0;
  font-size: 16px;
}

.property-price {
  font-size: 14px;
  font-weight: bold;
  color: #4CAF50;
  margin: 5px 0;
}

.property-details {
  display: flex;
  justify-content: space-between;
  color: #666;
  font-size: 12px;
}

.loading, .error {
  text-align: center;
  padding: 20px;
  color: #666;
}

.error {
  color: #ff4d4f; 
}
</style> 

 -->

















<!-- 
 its working perfect
 <template>
  <div class="location-detector">
    <div class="location-container">
    
      <button @click="handleButtonClick" class="location-btn" :disabled="isLoading">
        <span v-if="isLoading">Detecting...</span>
        <span v-else-if="currentLocation">{{ currentLocation }}</span>
        <span v-else>
          <i class="location-icon">📍</i> Detect Location
        </span>
      </button>

    
      <div v-if="showDropdown" class="dropdown">
      
        <div class="cities-grid">
          <div 
            v-for="city in cities" 
            :key="city.name" 
            class="city-item"
            @click="selectCity(city)"
          >
            {{ city.name }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'LocationDetector',
  data() {
    return {
      currentLocation: '',
      isLoading: false,
      error: null,
      showDropdown: false,
      cities: [ // Hardcoded cities
        { name: 'Delhi', properties: [] },
        { name: 'Mumbai', properties: [] },
        { name: 'Agra', properties: [] },
        { name: 'Jhansi', properties: [] },
        { name: 'Ludhiana', properties: [] },
      ],
    };
  },
  mounted() {
    // Automatically detect location when the component is mounted
    this.detectLocation();
  },
  methods: {
    handleButtonClick() {
      // Toggle dropdown visibility
      this.showDropdown = !this.showDropdown;
    },
    selectCity(city) {
      this.currentLocation = city.name; // Set the selected city
      this.showDropdown = false; // Close the dropdown
      this.$emit('location-detected', { city: city.name });
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
      console.log('Fetching city name for coordinates:', latitude, longitude); // Debugging

      fetch(`https://nominatim.openstreetmap.org/reverse?format=json&lat=${latitude}&lon=${longitude}&zoom=10`)
        .then(response => {
          if (!response.ok) {
            throw new Error('Failed to fetch city name');
          }
          return response.json();
        })
        .then(data => {
          console.log('API Response:', data); // Debugging

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
          console.error('Geocoding error:', error); // Debugging
          this.isLoading = false;
          this.error = "Failed to get city name";
          alert(this.error);
        });
    }
  }
};
</script>

<style scoped>
.location-detector {
  position: relative;
}

.location-btn {
  padding: 10px 20px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
}

.location-btn:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}

.dropdown {
  position: absolute;
  background-color: white;
  border: 1px solid #ccc;
  padding: 10px;
  z-index: 1000;
  width: 200px;
  max-height: 300px;
  overflow-y: auto;
  margin-top: 10px;
}

.cities-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(100px, 1fr));
  gap: 10px;
}

.city-item {
  padding: 5px;
  cursor: pointer;
  border: 1px solid #ddd;
  text-align: center;
}

.city-item:hover {
  background-color: #f0f0f0;
}
</style> -->





<template>
  <div class="location-detector">
    <div class="location-container">
      <!-- Button to detect location or open dropdown -->
      <button @click="handleButtonClick" class="location-btn" :disabled="isLoading">
        <span v-if="isLoading">Detecting...</span>
        <span v-else-if="currentLocation">{{ currentLocation }}</span>
        <span v-else>
          <i class="location-icon">📍</i> Detect Location
        </span>
      </button>

      <!-- Dropdown for cities -->
      <div v-if="showDropdown" class="dropdown">
        <!-- Show cities in a grid layout -->
        <div class="cities-grid">
          <div 
            v-for="city in cities" 
            :key="city.name" 
            class="city-item"
            @click="selectCity(city)"
          >
            {{ city.name }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'LocationDetector',
  data() {
    return {
      currentLocation: '',
      isLoading: false,
      error: null,
      showDropdown: false,
      cities: [ // Hardcoded cities
        { name: 'Delhi', properties: [] },
        { name: 'Mumbai', properties: [] },
        { name: 'Agra', properties: [] },
        { name: 'Jhansi', properties: [] },
        { name: 'Ludhiana', properties: [] },
      ],
    };
  },
  mounted() {
    // Automatically detect location when the component is mounted
    this.detectLocation();
  },
  methods: {
    handleButtonClick() {
      // Toggle dropdown visibility
      this.showDropdown = !this.showDropdown;
    },
    selectCity(city) {
      this.currentLocation = city.name; // Set the selected city
      this.showDropdown = false; // Close the dropdown
      this.$emit('location-detected', { city: city.name });
    },
    detectLocation() {
      this.isLoading = true;
      
      if (!navigator.geolocation) {
        this.error = "Geolocation is not supported by your browser";
        this.isLoading = false;
        alert(this.error);
        this.setDefaultLocation(); // Set default location if geolocation is not supported
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
          this.setDefaultLocation(); // Set default location if permission is denied
          break;
        case error.POSITION_UNAVAILABLE:
          this.error = "Location information is unavailable";
          this.setDefaultLocation(); // Set default location if location is unavailable
          break;
        case error.TIMEOUT:
          this.error = "The request to get user location timed out";
          this.setDefaultLocation(); // Set default location if the request times out
          break;
        default:
          this.error = "An unknown error occurred";
          this.setDefaultLocation(); // Set default location for any other error
          break;
      }
      alert(this.error);
    },
    getCityFromCoordinates(latitude, longitude) {
      console.log('Fetching city name for coordinates:', latitude, longitude); // Debugging

      fetch(`https://nominatim.openstreetmap.org/reverse?format=json&lat=${latitude}&lon=${longitude}&zoom=10`)
        .then(response => {
          if (!response.ok) {
            throw new Error('Failed to fetch city name');
          }
          return response.json();
        })
        .then(data => {
          console.log('API Response:', data); // Debugging

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
          console.error('Geocoding error:', error); // Debugging
          this.isLoading = false;
          this.error = "Failed to get city name";
          this.setDefaultLocation(); // Set default location if geocoding fails
          alert(this.error);
        });
    },
    setDefaultLocation() {
      // Set default location to Mumbai
      this.currentLocation = 'Mumbai';
      this.$emit('location-detected', { city: 'Mumbai' });
    }
  }
};
</script>

<style scoped>
.location-detector {
  position: relative;
}

.location-btn {
  padding: 10px 20px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
}

.location-btn:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}

.dropdown {
  position: absolute;
  background-color: white;
  border: 1px solid #ccc;
  padding: 10px;
  z-index: 1000;
  width: 200px;
  max-height: 300px;
  overflow-y: auto;
  margin-top: 10px;
}

.cities-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(100px, 1fr));
  gap: 10px;
}

.city-item {
  padding: 5px;
  cursor: pointer;
  border: 1px solid #ddd;
  text-align: center;
}

.city-item:hover {
  background-color: #f0f0f0;
}
</style>