<template>
  <div id="app">
    <Map :geoData="allRestaurantsData"></Map>
    <Sidebar @showWelcomePage="showWelcomePage" @showFilterByTagPage="showFilterByTagPage" />
    <transition name="slide-fade">
      <WelcomePage v-if="isWelcomePageActive" @emitHideCommand="isWelcomePageActive=false" />
      <FilterByTagPage v-else-if="isFilterByTagPageActive" @emitHideCommand="isFilterByTagPageActive=false"/>
    </transition>
   
  </div>
</template>

<script>
import Map from "./components/Map.vue";
import Sidebar from "./components/Sidebar.vue";
import WelcomePage from "./components/WelcomePage.vue";
import FilterByTagPage from "./components/FilterByTagPage.vue";
import axios from 'axios';
export default {
  name: "App",
  components: {
    Map,
    Sidebar,
    WelcomePage,
    FilterByTagPage,
  },
  data() {
    return {
      isWelcomePageActive: false,
      isFilterByTagPageActive: false,
      allRestaurantsData: []
    };
  },
  created(){
     axios
      .get("http://127.0.0.1:8000/api/v1/restaurants/")
      .then((response) => response.data)
      .then((data) => (this.allRestaurantsData = data))
      .catch((err) => alert(err));
  },
  mounted() {
    setTimeout(() => (this.isWelcomePageActive = false), 5000);
  },
  methods: {
    showWelcomePage() {
     this.isFilterByTagPageActive=false;
     this.isWelcomePageActive=true;
    },
    showFilterByTagPage() {
      this.isWelcomePageActive=false;
      this.isFilterByTagPageActive=true;
    },
  },
};
</script>
<style></style>
