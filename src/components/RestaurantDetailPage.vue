<template>
  <div id="detail-page">
    <div id="title">
      <h3>{{ data.features[0].properties.name }}</h3>
    </div>

    <div id="tags-list">
      <span
        class="each-tag"
        :key="tag.id"
        v-for="tag in data.features[0].properties.tags"
        ><img
          :src="iconBtc"
          class="icon-btc"
          v-if="tag.text.toLowerCase() === 'cryptocurrency'"
        />{{ tag.text | capitalize }}</span
      >
    </div>

    <div id="address">
      <b>{{ data.features[0].properties.address }}</b>
    </div>

    <div id="image">
      <img :src="data.features[0].properties.image_url" alt="" srcset="" />
    </div>

    <div id="subtitle">
      <p>{{ subtitle }}</p>
    </div>

    <div id="social">
      <a
        v-if="data.features[0].properties.url_main"
        :href="data.features[0].properties.url_main"
        target="_blank"
      >
        <img
          :src="globeHovered ? globeOrange : globeDark"
          @mouseover="globeHovered = true"
          @mouseleave="globeHovered = false"
        />
      </a>
      <a
        v-if="data.features[0].properties.url_facebook"
        :href="data.features[0].properties.url_facebook"
        target="_blank"
      >
        <img
          :src="facebookHovered ? facebookOrange : facebookDark"
          @mouseover="facebookHovered = true"
          @mouseleave="facebookHovered = false"
        />
      </a>

      <a
        v-if="data.features[0].properties.url_instagram"
        :href="data.features[0].properties.url_instagram"
        target="_blank"
      >
        <img
          :src="instagramHovered ? instagramOrange : instagramDark"
          @mouseover="instagramHovered = true"
          @mouseleave="instagramHovered = false"
        />
      </a>
      <a
        v-if="data.features[0].properties.url_twitter"
        :href="data.features[0].properties.url_twitter"
        target="_blank"
      >
        <img
          :src="twitterHovered ? twitterOrange : twitterDark"
          @mouseover="twitterHovered = true"
          @mouseleave="twitterHovered = false"
        />
      </a>
    </div>

    <div id="footer">
      <a
        v-if="data.features[0].properties.url_google_map"
        :href="data.features[0].properties.url_google_map"
        target="_blank"
        >{{ buttonText }}</a
      >
    </div>
    <button id="hide" @click.prevent="emitHideCommand">Hide</button>
  </div>
</template>

<script>
export default {
  props: ["data"],
  data() {
    return {
      language: localStorage.getItem("lang") || "en",
      globeDark: require("../assets/globe-dark.svg"),
      globeOrange: require("../assets/globe-orange.svg"),
      facebookDark: require("../assets/facebook-dark.svg"),
      facebookOrange: require("../assets/facebook-orange.svg"),
      twitterDark: require("../assets/twitter-dark.svg"),
      twitterOrange: require("../assets/twitter-orange.svg"),
      instagramDark: require("../assets/instagram-dark.svg"),
      instagramOrange: require("../assets/instagram-orange.svg"),
      iconBtc: require("../assets/btc.svg"),
      globeHovered: false,
      facebookHovered: false,
      twitterHovered: false,
      instagramHovered: false,
    };
  },
  created() {
    this.language = localStorage.getItem("lang");
  },

  computed: {
    subtitle: function() {
      switch (this.language) {
        case "en":
          return this.data.features[0].properties.description_en;
        case "ru":
          return this.data.features[0].properties.description_ru;
        case "ge":
          return this.data.features[0].properties.description_ge;
        default:
          return this.data.features[0].properties.description_en;
      }
    },
    buttonText: function() {
      switch (this.language) {
        case "en":
          return "Take me there";
        case "ru":
          return "Возьми меня туда";
        case "ge":
          return "წამიყვანე იქ";
        default:
          return "Take me there";
      }
    },
  },
  filters: {
    capitalize: function(value) {
      if (!value) return "";
      value = value.toString();
      return value.charAt(0).toUpperCase() + value.slice(1);
    },
  },
  methods: {
    emitHideCommand() {
      this.$emit("closeDetailCommand");
    },
  },
};
</script>

<style scoped>
#detail-page {
  background-color: white;
  width: 500px;
  height: calc(100vh - 140px);
  padding-left: 20px;
  padding-right: 25px !important;
  position: absolute;
  top: 140px;
  right: 0;
  z-index: 2;
  overflow-y: auto;
  overflow-x: hidden;
}

@media only screen and (max-width: 500px) {
  #detail-page {
    position: fixed;
    width: 100%;
  }
}

.title {
  font-size: 12px;
  font-family: "Poppins", sans-serif;
  line-height: 18px;
}

#tags-list {
  width: 100%;
}

.each-tag {
  display: inline-block;
  align-items: center;
  justify-content: center;
  padding: 1px 4px;
  font-family: "Montserrat", sans-serif;
  font-size: 12px;
  line-height: 14.63px;
  text-align: center;
  border: 1px solid black;
  border-radius: 4px;
  margin-right: 10px;
  white-space: nowrap;
  cursor: pointer;
}

.each-tag:hover {
  background-color: black;
  color: #ff7701;
}

.each-tag img {
  display: inline;
  margin-bottom: 1px;
}

#address {
  margin: 9px 0px;
}

#address b {
  font-size: 12px;
  font-weight: 700;
  font-family: "Poppins", sans-serif;
  line-height: 18px;
}

#hide {
  margin-top: 20px !important;
  font-weight: 200;
}

#subtitle p {
  font-weight: 300;
  line-height: 24px;
}
</style>
