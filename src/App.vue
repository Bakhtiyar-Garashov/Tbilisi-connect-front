<template>
  <div id="app">
    <Map :geo-data="allRestaurantsData" @emitObjectId="findObjectById"></Map>
    <Sidebar
      @showWelcomePage="showWelcomePage"
      @showFilterByTagPage="showFilterByTagPage"
      @showFilterByNamePage="receiveSearchWord"
      @resetSearchWord ="resetSearchWord"
    />
    <transition name="slide-fade">
      <WelcomePage
        v-if="isWelcomePageActive"
        @emitHideCommand="isWelcomePageActive = false"
      />
      <FilterByTagPage
        v-else-if="isFilterByTagPageActive"
        @emitHideCommand="isFilterByTagPageActive = false"
        @emitTagText="emitTagText"
      />
      <RestaurantDetailPage
        @closeDetailCommand="isRestaurantDetailPageActive = false"
        :data="allRestaurantsData"
        v-else-if="isRestaurantDetailPageActive"
      />

      <FilterByNamePage
        v-else-if="isFilterByNamePageActive"
        @closeFilterByNamePage="isFilterByNamePageActive = false"
        :searchWord="emittedWord"
        @emitObjectId="findObjectById"
      />
    </transition>
    <cookie-consent />
  </div>
</template>

<script>
import Map from "./components/Map.vue";
import Sidebar from "./components/Sidebar.vue";
import WelcomePage from "./components/WelcomePage.vue";
import FilterByTagPage from "./components/FilterByTagPage.vue";
import CookieConsent from "./components/CookieConsent.vue";
import RestaurantDetailPage from "./components/RestaurantDetailPage.vue";
import FilterByNamePage from "./components/FilterByNamePage.vue";
import axios from "axios";
export default {
  name: "App",
  components: {
    Map,
    Sidebar,
    WelcomePage,
    FilterByTagPage,
    CookieConsent,
    RestaurantDetailPage,
    FilterByNamePage,
  },
  data() {
    return {
      isWelcomePageActive: false,
      isFilterByTagPageActive: false,
      isRestaurantDetailPageActive: false,
      isFilterByNamePageActive: false,
      allRestaurantsData: [],
      emittedWord: "",
    };
  },
  created() {
    this.getAllData();
  },
  mounted() {
    setTimeout(() => (this.isWelcomePageActive = false), 5000);
  },
  methods: {
    showWelcomePage() {
      this.isFilterByTagPageActive = false;
      this.isWelcomePageActive = true;
    },
    showFilterByTagPage() {
      this.isWelcomePageActive = false;
      this.isFilterByTagPageActive = true;
    },
    emitTagText(value) {
      this.getDatabyTag(value);
    },
    receiveSearchWord(value) {
      this.isWelcomePageActive = false;
      this.isFilterByTagPageActive = false;
      this.isRestaurantDetailPageActive = false;
      this.isFilterByNamePageActive = true
      this.emittedWord = value;
    },
    resetSearchWord() {
      this.emittedWord = '';
    },
    async findObjectById(id) {
      try {
        const data = await axios.get(
          `https://api.bazaranet.co/api/v1/restaurants/${id}`
        );
        const singleData = {
          type: "FeatureCollection",
          features: [],
        };
        singleData.features.push(data.data);
        this.allRestaurantsData = singleData;
        this.isRestaurantDetailPageActive = true;
        this.isWelcomePageActive = false;
        this.isFilterByTagPageActive = false;
      } catch (error) {
        alert(`Error happened while fetching data. See more: ${error}`);
      }
    },

    async getAllData() {
      try {
        const data = await axios.get(
          "https://api.bazaranet.co/api/v1/restaurants/"
        );
        this.allRestaurantsData = data.data;
      } catch (error) {
        alert(`Error happened while fetching data. See more: ${error}`);
      }
    },

    async getDatabyTag(tag) {
      try {
        const data = await axios.get(
          `https://api.bazaranet.co/api/v1/restaurants/?tag=${tag}`
        );
        this.allRestaurantsData = data.data;
      } catch (error) {
        alert(`Error happened while fetching data. See more: ${error}`);
      }
    },
  },
  watch: {
    emittedWord: function(value) {
      if (value.length == 0) {
        this.isFilterByNamePageActive = false;
      }
    }
  },
};
</script>
<style></style>
