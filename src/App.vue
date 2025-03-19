
<template>
  <div class="real-estate-app">
    <!-- Header Section -->
    <header class="header">
      <div class="logo">MyRealEstate</div>
      <nav class="nav-menu">
        <a href="#" class="nav-link">Home</a>
        <a href="#" class="nav-link">Properties</a>
        <a href="#" class="nav-link">About</a>
        <a href="#" class="nav-link">Contact</a>
      </nav>
      <!-- LocationDetector Component -->
      <LocationDetector />
    </header>
  </div>
</template>

<script>
import LocationDetector from './LocationDetector.vue';

export default {
  name: 'App',
  components: {
    LocationDetector
  }
};
</script>

<style scoped>
.real-estate-app {
  font-family: Arial, sans-serif;
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

/* Header Section */
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 0;
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
  align-items: center;
}

.nav-link {
  text-decoration: none;
  color: #2c3e50;
  font-size: 16px;
}
</style>
