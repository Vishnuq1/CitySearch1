<!-- <template>
  <div class="real-estate-app">
    <header class="header">
      <div class="logo">MyRealEstate</div>
      <nav class="nav-menu">
        <a href="#" class="nav-link">Home</a>
        <a href="#" class="nav-link">Properties</a>
        <a href="#" class="nav-link">About</a>
        <a href="#" class="nav-link">Contact</a>
      </nav>
      <LocationDetector @location-detected="handleLocationDetection" />
    </header>
    
    <section class="banner-section">
      <div v-if="currentLocation" class="location-banner">
        <h1>Properties in {{ currentLocation }}</h1>
        <p>Find your dream home in {{ currentLocation }}</p>
      </div>
      <div v-else class="default-banner">
        <h1>Find Your Dream Home</h1>
        <p>Discover properties in your area by detecting your location</p>
      </div>
    </section>
    
    <section class="properties-section">
      <h2>{{ currentLocation ? `Properties in ${currentLocation}` : 'Featured Properties' }}</h2>
      
      <div v-if="isLoading" class="loading">
        Loading properties...
      </div>
      
      <div v-else-if="filteredProperties.length === 0" class="no-properties">
        No properties found in {{ currentLocation }}
      </div>
      
      <div v-else class="properties-grid">
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
import LocationDetector from './LocationDetector.vue';

export default {
  name: 'App',
  components: {
    LocationDetector
  },
  data() {
    return {
      currentLocation: '',
      isLoading: false,
      properties: [
        // This would normally come from your database
        // Sample data for demonstration
        {
          id: 1,
          title: 'Modern Apartment',
          city: 'New York',
          state: 'NY',
          price: 750000,
          bedrooms: 2,
          bathrooms: 2,
          sqft: 1200,
          image: 'https://via.placeholder.com/300x200'
        },
        {
          id: 2,
          title: 'Family Home',
          city: 'Los Angeles',
          state: 'CA',
          price: 1250000,
          bedrooms: 4,
          bathrooms: 3,
          sqft: 2400,
          image: 'https://via.placeholder.com/300x200'
        },
        {
          id: 3,
          title: 'Family Home',
          city: 'Gurgaon',
          state: 'Haryana',
          price: 1200,
          bedrooms: 40,
          bathrooms: 307,
          sqft: 24600,
          image: ''
        },
        {
          id: 4,
          title: 'Downtown Condo',
          city: 'Chicago',
          state: 'IL',
          price: 450000,
          bedrooms: 1,
          bathrooms: 1,
          sqft: 850,
          image: 'https://via.placeholder.com/300x200'
        },
        {
          id: 5,
          title: 'delhi',
          city: 'delhi',
          state: 'IL',
          price: 450000,
          bedrooms: 1,
          bathrooms: 1,
          sqft: 850,
          image: 'https://via.placeholder.com/300x200'
        },
        {
          id: 6,
          title: 'noida',
          city: 'up',
          state: 'IL',
          price: 450000,
          bedrooms: 1,
          bathrooms: 1,
          sqft: 850,
          image: 'https://via.placeholder.com/300x200'
        },
        {
          id: 7,
          title: 'gaziabad',
          city: 'up',
          state: 'IL',
          price: 450000,
          bedrooms: 1,
          bathrooms: 1,
          sqft: 850,
          image: 'https://via.placeholder.com/300x200'
        },
        // Add more sample properties as needed
      ]
    }
  },
  computed: {
    filteredProperties() {
      if (!this.currentLocation) {
        return this.properties; // Return all properties if no location is set
      }
      
      // Filter properties based on the current location
      return this.properties.filter(property => 
        property.city.toLowerCase() === this.currentLocation.toLowerCase() ||
        property.state.toLowerCase() === this.currentLocation.toLowerCase()
      );
    }
  },
  methods: {
    handleLocationDetection(locationData) {
      this.currentLocation = locationData.city;
      this.isLoading = true;
      
      // Simulate fetching data from database based on location
      setTimeout(() => {
        // In a real application, you would make an API call here
        // to fetch properties based on the detected location
        console.log('Fetched properties for:', locationData.city);
        this.isLoading = false;
      }, 1000);
    },
    
    // In a real application, you would have a method like this:
    fetchPropertiesByLocation(city) {
      // Example API call to your backend
      // fetch(`/api/properties?city=${city}`)
      //   .then(response => response.json())
      //   .then(data => {
      //     this.properties = data;
      //     this.isLoading = false;
      //   })
      //   .catch(error => {
      //     console.error('Error fetching properties:', error);
      //     this.isLoading = false;
      //   });
    }
  }
}
</script>

<style scoped>
.real-estate-app {
  font-family: Arial, sans-serif;
  max-width: 1200px;
  margin: 0 auto;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 0;
  border-bottom: 1px solid #eee;
}

.logo {
  font-size: 24px;
  font-weight: bold;
  color: #2c3e50;
}

.nav-menu {
  display: flex;
  gap: 20px;
}

.nav-link {
  text-decoration: none;
  color: #2c3e50;
}

.banner-section {
  padding: 60px 0;
  text-align: center;
  background-color: #f8f9fa;
  margin-bottom: 40px;
}

.properties-section {
  padding: 20px 0;
}

.properties-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 30px;
  margin-top: 30px;
}

.property-card {
  border: 1px solid #eee;
  border-radius: 8px;
  overflow: hidden;
  transition: transform 0.3s ease;
}

.property-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 20px rgba(0,0,0,0.1);
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
<!-- 
working but not detecting location automatically comment
<template>
  <div class="real-estate-app">
    <header class="header">
      <div class="logo">MyRealEstate</div>
      <nav class="nav-menu">
        <a href="#" class="nav-link">Home</a>
        <a href="#" class="nav-link">Properties</a>
        <a href="#" class="nav-link">About</a>
        <a href="#" class="nav-link">Contact</a>
      </nav>
      <LocationDetector 
        :cities="cities" 
        @location-detected="handleLocationDetection" 
      />
    </header>
    
  properties in different location comment
    <section class="properties-section">
      <h2>{{ currentLocation ? `Properties in ${currentLocation}` : 'Featured Properties' }}</h2>
      
      <div v-if="isLoading" class="loading">
        Loading properties...
      </div>
      
      <div v-else-if="filteredProperties.length === 0" class="no-properties">
        No properties found in {{ currentLocation }}
      </div>
      
      <div v-else class="properties-grid">
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
import LocationDetector from './LocationDetector.vue';

export default {
  name: 'App',
  components: {
    LocationDetector
  },
  data() {
    return {
      currentLocation: '',
      isLoading: false,
      cities: [
        'Gurgaon', 'Ghaziabad', 'Noida', 'New York', 
        'Delhi', 'Chicago', 'Los Angeles'
      ],
      properties: [
        // Your existing properties data
      ]
    };
  },
  computed: {
    filteredProperties() {
      if (!this.currentLocation) {
        return this.properties;
      }
      return this.properties.filter(property => 
        property.city.toLowerCase() === this.currentLocation.toLowerCase()
      );
    }
  },
  methods: {
    handleLocationDetection(locationData) {
      this.currentLocation = locationData.city;
      this.isLoading = true;
      
      setTimeout(() => {
        this.isLoading = false;
      }, 1000);
    }
  }
};
</script> -->




<template>
  <div class="real-estate-app">
    <header class="header">
      <div class="logo">MyRealEstate</div>
      <nav class="nav-menu">
        <a href="#" class="nav-link">Home</a>
        <a href="#" class="nav-link">Properties</a>
        <a href="#" class="nav-link">About</a>
        <a href="#" class="nav-link">Contact</a>
      </nav>
      <!-- Remove the :cities prop -->
      <LocationDetector @location-detected="handleLocationDetection" />
    </header>
    
    <!-- Display properties based on selected location -->
    <section class="properties-section">
      <h2>{{ currentLocation ? `Properties in ${currentLocation}` : 'Featured Properties' }}</h2>
      
      <div v-if="isLoading" class="loading">
        Loading properties...
      </div>
      
      <div v-else-if="filteredProperties.length === 0" class="no-properties">
        No properties found in {{ currentLocation }}
      </div>
      
      <div v-else class="properties-grid">
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
import LocationDetector from './LocationDetector.vue';

export default {
  name: 'App',
  components: {
    LocationDetector
  },
  data() {
    return {
      currentLocation: '',
      isLoading: false,
      properties: [
        // Your existing properties data
        {
          id: 1,
          title: 'Modern Apartment',
          city: 'New York',
          state: 'NY',
          price: 750000,
          bedrooms: 2,
          bathrooms: 2,
          sqft: 1200,
          image: 'https://via.placeholder.com/300x200'
        },
        {
          id: 2,
          title: 'Family Home',
          city: 'Los Angeles',
          state: 'CA',
          price: 1250000,
          bedrooms: 4,
          bathrooms: 3,
          sqft: 2400,
          image: 'https://via.placeholder.com/300x200'
        },
        {
          id: 3,
          title: 'Luxury Villa',
          city: 'Gurgaon',
          state: 'Haryana',
          price: 1200,
          bedrooms: 40,
          bathrooms: 307,
          sqft: 24600,
          image: 'https://via.placeholder.com/300x200'
        },
        {
          id: 4,
          title: 'Downtown Condo',
          city: 'Chicago',
          state: 'IL',
          price: 450000,
          bedrooms: 1,
          bathrooms: 1,
          sqft: 850,
          image: 'https://via.placeholder.com/300x200'
        },
        {
          id: 5,
          title: 'Delhi Apartment',
          city: 'Delhi',
          state: 'Delhi',
          price: 300000,
          bedrooms: 3,
          bathrooms: 2,
          sqft: 1500,
          image: 'https://via.placeholder.com/300x200'
        },
        {
          id: 6,
          title: 'Noida Villa',
          city: 'Noida',
          state: 'UP',
          price: 500000,
          bedrooms: 5,
          bathrooms: 4,
          sqft: 3000,
          image: 'https://via.placeholder.com/300x200'
        },
        {
          id: 7,
          title: 'Ghaziabad House',
          city: 'Ghaziabad',
          state: 'UP',
          price: 400000,
          bedrooms: 4,
          bathrooms: 3,
          sqft: 2500,
          image: 'https://via.placeholder.com/300x200'
        },
        {
          id: 8,
          title: 'Gurgaon',
          city: 'phase 3',
          state: 'Haryana',
          price: 400,
          bedrooms: 4,
          bathrooms: 3,
          sqft: 2500,
          image: 'https://via.placeholder.com/300x200'
        }
      ]
    };
  },
  computed: {
    filteredProperties() {
      if (!this.currentLocation) {
        return this.properties; // Show all properties if no location is selected
      }
      
      // Filter properties based on the selected city
      return this.properties.filter(property => 
        property.city.toLowerCase() === this.currentLocation.toLowerCase()
      );
    }
  },
  methods: {
    handleLocationDetection(locationData) {
      this.currentLocation = locationData.city;
      this.isLoading = true;
      
      // Simulate fetching data from an API
      setTimeout(() => {
        this.isLoading = false;
      }, 1000);
    }
  }
};
</script>

<style scoped>
.real-estate-app {
  font-family: Arial, sans-serif;
  max-width: 1200px;
  margin: 0 auto;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 0;
  border-bottom: 1px solid #eee;
}

.logo {
  font-size: 24px;
  font-weight: bold;
  color: #2c3e50;
}

.nav-menu {
  display: flex;
  gap: 20px;
}

.nav-link {
  text-decoration: none;
  color: #2c3e50;
}

.properties-section {
  padding: 20px 0;
}

.properties-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 30px;
  margin-top: 30px;
}

.property-card {
  border: 1px solid #eee;
  border-radius: 8px;
  overflow: hidden;
  transition: transform 0.3s ease;
}

.property-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 20px rgba(0,0,0,0.1);
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