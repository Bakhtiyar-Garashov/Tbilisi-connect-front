<template>
  <div id="map"></div>
</template>

<script>
import mapboxgl from "mapbox-gl";
import "mapbox-gl/dist/mapbox-gl.css";

export default {
  components: {},
  data() {
    return {
      accessToken:
        "pk.eyJ1IjoiYmFraHRpeWFyZ2FyYXNob3YiLCJhIjoiY2tydDQ4OXQ0MWRkNzJ0cGZ4Y2dqOTU1NiJ9.bXbuhTIaTwLV39SNY9B5VQ", // your access token. Needed if you using Mapbox maps
      mapStyle: "mapbox://styles/bakhtiyargarashov/ckrt8nouh2o1117pii8nhz1o2", // your map style
      center: [44.80293273925781, 
                41.6944496643259],
      map: {},
    };
  },

  methods: {
    async createMap() {
      try {
        mapboxgl.accessToken = this.accessToken;
        const map = new mapboxgl.Map({
          container: "map",
          style: this.mapStyle,
          center: this.center,
          zoom: 9,
        });
        map.addControl(new mapboxgl.NavigationControl(), "bottom-left");
        this.map = map;
      } catch (err) {
        alert(`Error occurred when adding map. See more: ${err}`);
      }
    },
  },

  mounted() {
    this.createMap();
    // Zoom to the zoom level 8 with an animated transition
    this.map.zoomTo(11, {
      duration: 1200,
    });
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#map {
  height: 100vh;
  width: 100vw;
}
</style>
