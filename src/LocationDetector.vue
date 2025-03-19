










<!-- 

working perfectly fine 


<template>
  <div class="location-detector">

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
          v-for="city in availableCities" 
          :key="city.name" 
          class="city-item"
          @click="selectCity(city)"
        >
          {{ city.name }}
        </div>
      </div>
    </div>


    <section class="properties-section" v-if="showProperties">
      <h2>{{ currentLocation ? `Properties in ${currentLocation}` : 'Featured Properties' }}</h2>
      
      <div v-if="isLoadingProperties" class="loading">
        Loading properties...
      </div>
      
      <div v-else-if="filteredProperties.length === 0" class="no-properties">
        No properties found in {{ currentLocation }}
      </div>
      
   
      <div v-else class="properties-horizontal-scroll">
        <div v-for="property in filteredProperties" :key="property.id" class="property-card">
          <img :src="property.image" :alt="property.title" class="property-image">
          <div class="property-info">
            <h3>{{ property.title }}</h3>
            <p class="property-location">{{ property.city }}, {{ property.state }}</p>
            <p class="property-price">${{ property.price.toLocaleString() }}</p>
            <div class="property-details">
              <span>{{ property.bedrooms }} beds</span>
              <span>{{ property.bathrooms }} baths</span>
              <span>{{ property.sqft.toLocaleString() }} sqft</span>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>
<script>

export default {
  name: 'LocationDetector',
  data() {
    return {
      currentLocation: '',
      isLoading: false,
      isLoadingProperties: false,
      error: null,
      showDropdown: false,
      showProperties: true,
      properties: [
        {
          id: 1,
          title: 'Modern Apartment',
          city: 'Mumbai',
          state: 'Maharashtra',
          price: 750000,
          bedrooms: 2,
          bathrooms: 2,
          sqft: 1200,
          image: 'https://via.placeholder.com/300x200'
        },
        {
          id: 2,
          title: 'Luxury Villa',
          city: 'Mumbai',
          state: 'Maharashtra',
          price: 1200000,
          bedrooms: 4,
          bathrooms: 3,
          sqft: 2400,
          image: 'https://via.placeholder.com/300x200'
        },
        {
          id: 3,
          title: 'Downtown Condo',
          city: 'Delhi',
          state: 'Delhi',
          price: 450000,
          bedrooms: 1,
          bathrooms: 1,
          sqft: 850,
          image: 'https://via.placeholder.com/300x200'
        },
        {
          id: 4,
          title: 'Beach House',
          city: 'Bangalore',
          state: 'Karnataka',
          price: 900000,
          bedrooms: 3,
          bathrooms: 2,
          sqft: 1800,
          image: 'https://via.placeholder.com/300x200'
        },
        {
          id: 5,
          title: 'Beac',
          city: 'Gurgaon',
          state: 'Haryana',
          price: 900000,
          bedrooms: 3,
          bathrooms: 2,
          sqft: 1800,
          image: 'https://via.placeholder.com/300x200'
        },
        {
          id: 6,
          title: 'House',
          city: 'Jhansi',
          state: 'UP',
          price: 900000,
          bedrooms: 3,
          bathrooms: 2,
          sqft: 1800,
          image: 'https://via.placeholder.com/300x200'
        },
        {
          id: 7,
          title: 'Beause',
          city: 'Faridabad',
          state: 'Haryana',
          price: 900000,
          bedrooms: 3,
          bathrooms: 2,
          sqft: 1800,
          image: 'https://via.placeholder.com/300x200'
        }
      ],
      availableCities: [
        { name: 'Mumbai', id: 'mumbai' },
        { name: 'Delhi', id: 'delhi' },
        { name: 'Bangalore', id: 'bangalore' },
        { name: 'Gurgaon', id: 'gurgaon' },
        { name: 'Jhansi', id: 'jhansi' },
        { name: 'Faridabad', id: 'faridabad' }
      ]
    };
  },
  computed: {
    filteredProperties() {
      if (!this.currentLocation) return this.properties;
      return this.properties.filter(property => 
        property.city.toLowerCase() === this.currentLocation.toLowerCase()
      );
    }
  },
  mounted() {
    this.detectLocation(); // Automatically detect location on component mount
  },
  methods: {
    handleButtonClick() {
      this.showDropdown = !this.showDropdown; // Toggle dropdown
    },
    selectCity(city) {
      this.currentLocation = city.name; // Update selected city
      this.showDropdown = false; // Close dropdown
    },
    detectLocation() {
      this.isLoading = true;
      if (!navigator.geolocation) {
        this.error = "Geolocation is not supported by your browser";
        this.isLoading = false;
        this.setDefaultLocation();
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
      this.getCityFromCoordinates(latitude, longitude);
    },
    handleLocationError(error) {
      this.isLoading = false;
      this.error = "Failed to detect location";
      this.setDefaultLocation();
    },
    async getCityFromCoordinates(latitude, longitude) {
      try {
        const response = await fetch(
          `https://nominatim.openstreetmap.org/reverse?format=json&lat=${latitude}&lon=${longitude}&zoom=10`
        );
        const data = await response.json();
        this.currentLocation = data.address.city || data.address.town || data.address.village || "Unknown";
        this.isLoading = false;
      } catch (error) {
        console.error('Error fetching city:', error);
        this.setDefaultLocation();
      }
    },
    setDefaultLocation() {
      this.currentLocation = 'Mumbai';
    }
  }
};
</script>

<style scoped>
.location-detector {
  margin-top: 20px;
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

.properties-section {
  margin-top: 20px;
}

.properties-section h2 {
  font-size: 22px;
  margin-bottom: 20px;
}

.properties-horizontal-scroll {
  display: flex;
  overflow-x: auto;
  gap: 20px;
  padding: 10px 0;
}

.property-card {
  flex: 0 0 auto;
  width: 300px;
  border: 1px solid #eee;
  border-radius: 8px;
  overflow: hidden;
  transition: transform 0.3s ease;
}

.property-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
}

.property-image {
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.property-info {
  padding: 15px;
}

.property-location {
  color: #666;
  margin: 5px 0;
}

.property-price {
  font-size: 18px;
  font-weight: bold;
  color: #4CAF50;
  margin: 10px 0;
}

.property-details {
  display: flex;
  justify-content: space-between;
  color: #666;
  font-size: 14px;
}

.loading, .no-properties {
  text-align: center;
  padding: 40px;
  color: #666;
}
</style> -->

<template>
  <div class="location-detector">
    <!-- Location Button -->
    <button @click="handleButtonClick" class="location-btn" :disabled="isLoading">
      <span v-if="isLoading">Detecting...</span>
      <span v-else-if="currentLocation">{{ currentLocation }}</span>
      <span v-else>
        <i class="location-icon">📍</i> Detect Location
      </span>
    </button>

    <!-- Dropdown for Cities -->
    <div v-if="showDropdown" class="dropdown">
      <div class="cities-grid" :style="gridStyle">
        <div 
          v-for="city in availableCities" 
          :key="city.name" 
          class="city-item"
          @click="selectCity(city)"
        >
          {{ city.name }}
        </div>
      </div>
    </div>

    <!-- Properties Section -->
    <section class="properties-section" v-if="showProperties">
      <h2>{{ currentLocation ? `Properties in ${currentLocation}` : 'Featured Properties' }}</h2>
      
      <div v-if="isLoadingProperties" class="loading">
        Loading properties...
      </div>
      
      <div v-else-if="filteredProperties.length === 0" class="no-properties">
        No properties found in {{ currentLocation }}
      </div>
      
      <!-- Horizontal Scrollable Container for Properties -->
      <div v-else class="properties-horizontal-scroll">
        <div v-for="property in filteredProperties" :key="property.id" class="property-card">
          <img :src="property.image" :alt="property.title" class="property-image">
          <div class="property-info">
            <h3>{{ property.title }}</h3>
            <p class="property-location">{{ property.city }}, {{ property.state }}</p>
            <p class="property-price">${{ property.price.toLocaleString() }}</p>
            <div class="property-details">
              <span>{{ property.bedrooms }} beds</span>
              <span>{{ property.bathrooms }} baths</span>
              <span>{{ property.sqft.toLocaleString() }} sqft</span>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: 'LocationDetector',
  data() {
    return {
      currentLocation: '',
      isLoading: false,
      isLoadingProperties: false,
      error: null,
      showDropdown: false,
      showProperties: true,
      // Hardcoded properties data - TO BE REMOVED WHEN API IS INTEGRATED
      properties: [
        {
          id: 1,
          title: 'Modern Apartment',
          city: 'Mumbai',
          state: 'Maharashtra',
          price: 750000,
          bedrooms: 2,
          bathrooms: 2,
          sqft: 1200,
          image: 'https://via.placeholder.com/300x200'
        },
        {
          id: 2,
          title: 'Luxury Villa',
          city: 'Mumbai',
          state: 'Maharashtra',
          price: 1200000,
          bedrooms: 4,
          bathrooms: 3,
          sqft: 2400,
          image: 'https://via.placeholder.com/300x200'
        },
        // Add more properties as needed
      ],
      // Hardcoded cities data - TO BE REMOVED WHEN API IS INTEGRATED
      availableCities: [
        { name: 'Mumbai', id: 'mumbai' },
        { name: 'Delhi', id: 'delhi' },
        { name: 'Bangalore', id: 'bangalore' },
        { name: 'Gurgaon', id: 'gurgaon' },
        { name: 'Jhansi', id: 'jhansi' },
        { name: 'Faridabad', id: 'faridabad' }
      ]
    };
  },
  computed: {
    filteredProperties() {
      if (!this.currentLocation) return this.properties;
      return this.properties.filter(property => 
        property.city.toLowerCase() === this.currentLocation.toLowerCase()
      );
    },
    gridStyle() {
      const numCities = this.availableCities.length;
      let columns = Math.floor(Math.sqrt(numCities));
      let rows = Math.ceil(numCities / columns);

      // Ensure rows are always more than columns
      while (rows <= columns) {
        columns--;
        rows = Math.ceil(numCities / columns);
      }

      return {
        'grid-template-columns': `repeat(${columns}, 1fr)`
      };
    }
  },
  mounted() {
    this.detectLocation(); // Automatically detect location on component mount
    // TODO: Fetch cities and properties from API when the component is mounted
    // this.fetchCities();
    // this.fetchProperties();
  },
  methods: {
    handleButtonClick() {
      this.showDropdown = !this.showDropdown; // Toggle dropdown
    },
    selectCity(city) {
      this.currentLocation = city.name; // Update selected city
      this.showDropdown = false; // Close dropdown
      // TODO: Fetch properties for the selected city from API
      // this.fetchPropertiesByCity(city.id);
    },
    detectLocation() {
      this.isLoading = true;
      if (!navigator.geolocation) {
        this.error = "Geolocation is not supported by your browser";
        this.isLoading = false;
        this.setDefaultLocation();
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
      this.getCityFromCoordinates(latitude, longitude);
    },
    handleLocationError(error) {
      this.isLoading = false;
      this.error = "Failed to detect location";
      this.setDefaultLocation();
    },
    async getCityFromCoordinates(latitude, longitude) {
      try {
        const response = await fetch(
          `https://nominatim.openstreetmap.org/reverse?format=json&lat=${latitude}&lon=${longitude}&zoom=10`
        );
        const data = await response.json();
        this.currentLocation = data.address.city || data.address.town || data.address.village || "Unknown";
        this.isLoading = false;
        // TODO: Fetch properties for the detected city from API
        // this.fetchPropertiesByCity(this.currentLocation);
      } catch (error) {
        console.error('Error fetching city:', error);
        this.setDefaultLocation();
      }
    },
    setDefaultLocation() {
      this.currentLocation = 'Mumbai';
    },
    // TODO: Add methods to fetch cities and properties from API
    // async fetchCities() {
    //   try {
    //     const response = await axios.get('/api/cities');
    //     this.availableCities = response.data;
    //   } catch (error) {
    //     console.error('Error fetching cities:', error);
    //   }
    // },
    // async fetchProperties() {
    //   try {
    //     const response = await axios.get('/api/properties');
    //     this.properties = response.data;
    //   } catch (error) {
    //     console.error('Error fetching properties:', error);
    //   }
    // },
    // async fetchPropertiesByCity(cityId) {
    //   try {
    //     const response = await axios.get(`/api/properties?city=${cityId}`);
    //     this.properties = response.data;
    //   } catch (error) {
    //     console.error('Error fetching properties by city:', error);
    //   }
    // }
  }
};
</script>

<style scoped>
.location-detector {
  margin-top: 20px;
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

.properties-section {
  margin-top: 20px;
}

.properties-section h2 {
  font-size: 22px;
  margin-bottom: 20px;
}

.properties-horizontal-scroll {
  display: flex;
  overflow-x: auto;
  gap: 20px;
  padding: 10px 0;
}

.property-card {
  flex: 0 0 auto;
  width: 300px;
  border: 1px solid #eee;
  border-radius: 8px;
  overflow: hidden;
  transition: transform 0.3s ease;
}

.property-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
}

.property-image {
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.property-info {
  padding: 15px;
}

.property-location {
  color: #666;
  margin: 5px 0;
}

.property-price {
  font-size: 18px;
  font-weight: bold;
  color: #4CAF50;
  margin: 10px 0;
}

.property-details {
  display: flex;
  justify-content: space-between;
  color: #666;
  font-size: 14px;
}

.loading, .no-properties {
  text-align: center;
  padding: 40px;
  color: #666;
}
</style>