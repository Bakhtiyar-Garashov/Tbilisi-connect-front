<template>
  <div id="app">
    <Map :geo-data="allRestaurantsData" @emitObjectId="findObjectById"></Map>
    <Sidebar @showWelcomePage="showWelcomePage" @showFilterByTagPage="showFilterByTagPage" />
    <transition name="slide-fade">
      <WelcomePage v-if="isWelcomePageActive" @emitHideCommand="isWelcomePageActive=false" />
      <FilterByTagPage v-else-if="isFilterByTagPageActive" @emitHideCommand="isFilterByTagPageActive=false" @emitTagText="emitTagText"/>
      <RestaurantDetailPage :one-restaurant-data="oneRestaurantData" v-else-if="isRestaurantDetailPageActive"/>
    </transition>
   <cookie-consent/>
  </div>
</template>

<script>
import Map from "./components/Map.vue";
import Sidebar from "./components/Sidebar.vue";
import WelcomePage from "./components/WelcomePage.vue";
import FilterByTagPage from "./components/FilterByTagPage.vue";
import CookieConsent from './components/CookieConsent.vue';
import RestaurantDetailPage from './components/RestaurantDetailPage.vue';
import axios from 'axios';
export default {
  name: "App",
  components: {
    Map,
    Sidebar,
    WelcomePage,
    FilterByTagPage,
    CookieConsent,
    RestaurantDetailPage,
  },
  data() {
    return {
      isWelcomePageActive: false,
      isFilterByTagPageActive: false,
      isRestaurantDetailPageActive: false,
      allRestaurantsData: [],
      oneRestaurantData:[],
    };
  },
  created(){
     this.getAllData();
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
    emitTagText(value){
      this.getDatabyTag(value);
    },

    async findObjectById(id){
      this.isRestaurantDetailPageActive = true;
      try {
        const data = await axios.get(`http://127.0.0.1:8000/api/v1/restaurants/${id}`);
        this.oneRestaurantData = data.data;
      } catch (error){
        alert(`Error happened while fetching data. See more: ${error}`)
      }
    },

    async getAllData(){
      try {
        const data = await axios.get("http://127.0.0.1:8000/api/v1/restaurants/");
        this.allRestaurantsData = data.data;
      } catch (error) {
        alert(`Error happened while fetching data. See more: ${error}`)
      }
    },

    async getDatabyTag(tag){
      try {
        const data = await axios.get(`http://127.0.0.1:8000/api/v1/restaurants/?tag=${tag}`);
        this.allRestaurantsData = data.data;
      } catch (error){
        alert(`Error happened while fetching data. See more: ${error}`)
      }
    }
  },
};
</script>
<style></style>
