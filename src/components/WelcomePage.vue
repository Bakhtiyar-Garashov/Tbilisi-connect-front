<template>
  <div id="welcome-page">
    <div id="title">
      <h3 class="title">{{ welcomePageData.title }}</h3>
    </div>
    <div id="image">
      <img :src="welcomePageData.image_url" alt="" srcset="" />
    </div>
    <div id="subtitle">
      <p>{{ subtitle }}</p>
    </div>
    <div id="social">
      <a
        v-if="welcomePageData.url_main"
        :href="welcomePageData.url_main"
        target="_blank"
      >
        <img
          :src="globeHovered ? globeOrange : globeDark"
          @mouseover="globeHovered = true"
          @mouseleave="globeHovered = false"
        />
      </a>
      <a
        v-if="welcomePageData.url_facebook"
        :href="welcomePageData.url_facebook"
        target="_blank"
      >
        <img
          :src="facebookHovered ? facebookOrange : facebookDark"
          @mouseover="facebookHovered = true"
          @mouseleave="facebookHovered = false"
        />
      </a>

      <a
        v-if="welcomePageData.url_instagram"
        :href="welcomePageData.url_instagram"
        target="_blank"
      >
        <img
          :src="instagramHovered ? instagramOrange : instagramDark"
          @mouseover="instagramHovered = true"
          @mouseleave="instagramHovered = false"
        />
      </a>
      <a
        v-if="welcomePageData.url_twitter"
        :href="welcomePageData.url_twitter"
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
        v-if="welcomePageData.url_facebook"
        :href="welcomePageData.url_facebook"
        target="_blank"
        >{{ buttonText }}</a
      >
    </div>
    <button id="hide" @click.prevent="emitHideCommand">Hide</button>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      welcomePageData: [],
      globeDark: require("../assets/globe-dark.svg"),
      globeOrange: require("../assets/globe-orange.svg"),
      facebookDark: require("../assets/facebook-dark.svg"),
      facebookOrange: require("../assets/facebook-orange.svg"),
      twitterDark: require("../assets/twitter-dark.svg"),
      twitterOrange: require("../assets/twitter-orange.svg"),
      instagramDark: require("../assets/instagram-dark.svg"),
      instagramOrange: require("../assets/instagram-orange.svg"),
      globeHovered: false,
      facebookHovered: false,
      twitterHovered: false,
      instagramHovered: false,
      language: localStorage.getItem("lang") || "en",
    };
  },
  created() {
    this.getData();
    this.language = localStorage.getItem("lang");
  },
  methods: {
    emitHideCommand() {
      this.$emit("emitHideCommand");
    },
    
    async getData() {
      try {
        const data = await axios.get("http://127.0.0.1:8000/api/v1/welcome/");
        this.welcomePageData = data.data[0];
      } catch (error) {
        alert(`Error occurred. See: ${error}`);
      }
    },
  },

  computed: {
    subtitle: function() {
      switch (this.language) {
        case "en":
          return this.welcomePageData.subtitle_en;
        case "ru":
          return this.welcomePageData.subtitle_ru;
        case "ge":
          return this.welcomePageData.subtitle_ge;
        default:
          return this.welcomePageData.subtitle_en;
      }
    },
    buttonText: function() {
      switch (this.language) {
        case "en":
          return "Follow us on Facebook";
        case "ru":
          return "Следите за нами на Фейсбуке";
        case "ge":
          return "მოგვყევით ფეისბუქზე";
        default:
          return "Follow us on Facebook";
      }
    },
  },
};
</script>

<style>
#welcome-page {
  background-color: white;
  width: 500px;
  height: calc(100vh - 140px);
  padding-left: 20px;
  padding-right: 40px;
  position: absolute;
  top: 140px;
  right: 0;
  overflow-y: auto;
  overflow-x: hidden;
}
#title {
  font-size: 20px;
  font-weight: 900 !important;
  line-height: 30px;
  margin-top: 26px;
  margin-bottom: 8px;
  color: black;
}
#subtitle {
  margin-top: 10px;
  font-size: 16px;
  line-height: 24px;
  color: black;
  font-weight: 700;
}
#image img {
  width: 440px;
  height: 288px;
  object-fit: cover;
  border-radius: 4px;
}

#footer {
  margin-top: 26px;
  padding-bottom: 50px;
}

#footer a {
  border: 1px solid black;
  border-radius: 4px;
  font-weight: 700;
  line-height: 14.63px;
  color: black;
  font-family: "Montserrat", sans-serif;
  padding: 6px 10px;
}
#footer a:hover {
  background-color: black;
  color: #ff7701;
}

#social {
  display: flex;
  flex-direction: row;
  align-items: center;
}

#social a {
  display: inline;
  margin-right: 11.51px;
  margin-top: 28px;
}

#hide {
  right: 40px;
  font-weight: 500;
  margin-top: 10vh;
  margin-bottom: 20px;
  float: right;
}
</style>
