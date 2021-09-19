<template>
  <div id="map"></div>
</template>

<script>
import mapboxgl from "mapbox-gl";
import "mapbox-gl/dist/mapbox-gl.css";
import markerIcon from "../assets/Group.png";
export default {
  props: {
    geoData: {
      required: true,
    },
  },
  components: {},
  data() {
    return {
      accessToken:
        "pk.eyJ1IjoiYmFraHRpeWFyZ2FyYXNob3YiLCJhIjoiY2tydDQ4OXQ0MWRkNzJ0cGZ4Y2dqOTU1NiJ9.bXbuhTIaTwLV39SNY9B5VQ", // your access token. Needed if you using Mapbox maps
      mapStyle: "mapbox://styles/bakhtiyargarashov/ckrt8nouh2o1117pii8nhz1o2", // your map style
      center: [44.890641, 41.712012],
      map: {},
    };
  },

  methods: {
    createMap() {
      try {
        mapboxgl.accessToken = this.accessToken;
        const map = new mapboxgl.Map({
          container: "map",
          style: this.mapStyle,
          center: this.center,
          zoom: 10,
        });
        map.addControl(new mapboxgl.NavigationControl(), "bottom-left");
        this.map = map;
      } catch (err) {
        alert(`Error occurred when adding map. See more: ${err}`);
      }
    },

    emitObjectId(id) {
      this.$emit("emitObjectId", id);
    },
  },

  watch: {
    geoData: function() {
      this.map.getSource("all_restaurants").setData(this.geoData);
    },
  },

  mounted() {
    this.createMap();
    // Zoom to the zoom level with an animated transition
    this.map.flyTo({
      // These options control the ending camera position: centered at
      // the target, at zoom level 9, and north up.
      center: [44.860701, 41.716257],
      zoom: 11,
      bearing: 0,

      // These options control the flight curve, making it move
      // slowly and zoom out almost completely before starting
      // to pan.
      speed: 0.5, // make the flying slow
      curve: 1, // change the speed at which it zooms out

      // This can be any easing function: it takes a number between
      // 0 and 1 and returns another number between 0 and 1.
      easing: (t) => t,

      // this animation is considered essential with respect to prefers-reduced-motion
      essential: true,
    });

    let img = new Image(47, 45.5);
    img.onload = () => this.map.addImage("icon", img);
    img.src = markerIcon;

    this.map.on("load", () => {
      this.map.addSource("all_restaurants", {
        type: "geojson",
        data: this.geoData,
        cluster: true,
        clusterRadius: 80,
      });
      // adds layer

      this.map.addLayer({
        id: "clusters",
        type: "circle",
        source: "all_restaurants",
        filter: ["has", "point_count"],
        paint: {
          // Use step expressions (https://docs.mapbox.com/mapbox-gl-js/style-spec/#expressions-step)
          // with three steps to implement three types of circles:
          //   * Blue, 20px circles when point count is less than 100
          //   * Yellow, 30px circles when point count is between 100 and 750
          //   * Pink, 40px circles when point count is greater than or equal to 750
          "circle-color": [
            "step",
            ["get", "point_count"],
            "#FA7701",
            100,
            "#FA7701",
            750,
            "#FA7701",
          ],
          "circle-radius": [
            "step",
            ["get", "point_count"],
            20,
            100,
            20,
            750,
            20,
          ],
        },
      });

      // count of points inside cluster
      this.map.addLayer({
        id: "cluster-count",
        type: "symbol",
        source: "all_restaurants",
        filter: ["has", "point_count"],
        paint: {
          "text-color": "#fff",
        },
        layout: {
          "text-field": "{point_count_abbreviated}",
          "text-font": ["DIN Offc Pro Medium", "Arial Unicode MS Bold"],
          "text-size": 20,
        },
      });

      // unclustered point objects

      this.map.addLayer({
        id: "unclustered-point",
        type: "symbol",
        source: "all_restaurants",
        filter: ["!", ["has", "point_count"]],

        layout: {
          "icon-image": "icon",
          "icon-size": 1,
        },
      });

      // inspect a cluster on click
      this.map.on("click", "clusters", (e) => {
        const features = this.map.queryRenderedFeatures(e.point, {
          layers: ["clusters"],
        });
        const clusterId = features[0].properties.cluster_id;
        this.map
          .getSource("all_restaurants")
          .getClusterExpansionZoom(clusterId, (err, zoom) => {
            if (err) return;

            this.map.easeTo({
              center: features[0].geometry.coordinates,
              zoom: zoom,
            });
          });
      });

      const popup = new mapboxgl.Popup({
        closeButton: false,
        closeOnClick: false,
        className: "mapboxgl-popup-custom",
      });

      // When a click event occurs on a feature in
      // the unclustered-point layer, open a popup at
      // the location of the feature, with
      // description HTML from its properties.
      this.map.on("mousemove", "unclustered-point", (e) => {
        const coordinates = e.features[0].geometry.coordinates.slice();
        const name = e.features[0].properties.name;

        // Ensure that if the map is zoomed out such that
        // multiple copies of the feature are visible, the
        // popup appears over the copy being pointed to.
        while (Math.abs(e.lngLat.lng - coordinates[0]) > 180) {
          coordinates[0] += e.lngLat.lng > coordinates[0] ? 360 : -360;
        }
        this.map.getCanvas().style.cursor = "pointer";
        this.map.setLayoutProperty("unclustered-point", "icon-size", [
          "case",
          ["==", ["get", "name"], e.features[0].properties.name],
          1.2,
          1,
        ]);

        popup
          .setLngLat(coordinates)
          .setHTML(name)
          .addTo(this.map);
      });

      this.map.on("click", "unclustered-point", (e) => {
        this.emitObjectId(e.features[0].id);
      });

      this.map.on("mouseleave", "unclustered-point", () => {
        this.map.setLayoutProperty("unclustered-point", "icon-size", 1);
        popup.remove();
        this.map.getCanvas().style.cursor = "pointer";
      });

      this.map.on("mousemove", "clusters", () => {
        this.map.getCanvas().style.cursor = "pointer";
      });
      this.map.on("mouseleave", "clusters", () => {
        this.map.getCanvas().style.cursor = "";
      });
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
