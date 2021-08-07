<template>
  <div id="welcome-page">
    <div id="title">
      <h3 class="title">{{ welcomePageData.title }}</h3>
    </div>
    <div id="image">
      <img :src="welcomePageData.image_url" alt="" srcset="" />
    </div>
    <div id="subtitle">
      <p>{{ welcomePageData.subtitle_en }}</p>
    </div>
    <div id="social">
        <a v-if="welcomePageData.url_main" :href="welcomePageData.url_main" target="_blank">
        <v-icon v-if="welcomePageData.url_main" class="web">{{ web }}</v-icon>
      </a>
      <a v-if="welcomePageData.url_facebook" :href="welcomePageData.url_facebook" target="_blank">
        <v-icon v-if="welcomePageData.url_facebook" class="facebook">{{ faceboook }}</v-icon>
      </a>
      <a v-if="welcomePageData.url_instagram" :href="welcomePageData.url_instagram" target="_blank">
        <v-icon v-if="welcomePageData.url_instagram" class="web">{{ instagram }}</v-icon>
      </a>
      <a v-if="welcomePageData.url_twitter" :href="welcomePageData.url_twitter" target="_blank">
        <v-icon v-if="welcomePageData.url_twitter" class="web">{{ twitter }}</v-icon>
      </a>
    </div>
    <div id="footer">
     <a v-if="welcomePageData.url_facebook" :href="welcomePageData.url_facebook" target="_blank">Follow us on Facebook</a>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { mdiEarth, mdiFacebook, mdiInstagram, mdiTwitter } from "@mdi/js";
export default {
  data() {
    return {
      welcomePageData: [],
      web: mdiEarth,
      faceboook: mdiFacebook,
      instagram: mdiInstagram,
      twitter: mdiTwitter,
    };
  },
  created() {
    axios
      .get("http://127.0.0.1:8000/api/v1/welcome/")
      .then((response) => (this.welcomePageData = response.data[0]))
      .catch((err) => alert(err));
  },
};
</script>

<style>
#welcome-page {
  background-color: white;
  width: 500px;
  height: 100%;
  padding-left: 20px;
  padding-right: 40px;
}
#title {
  font-size: 20px;
  font-weight: 700;
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
  font-weight: 400;
}
#image img {
  width: 440px;
  height: 288px;
  object-fit: cover;
  border-radius: 4px;
}
.web,
.instagram,
.facebook,
.twitter {
  margin-right: 11.51px;
  margin-top: 28px;
}

#footer {
    margin-top: 26px;
    padding-bottom: 50px;
}

#footer a {
    border: 1px solid black;
    border-radius: 4px;
    font-weight: 500;
    line-height: 14.63px;
    color: black;
    font-family: 'Montserrat', sans-serif;
    padding: 10px 6px;
}
</style>
