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
      <a
        v-if="welcomePageData.url_main"
        :href="welcomePageData.url_main"
        target="_blank"
      >
        <img v-if="welcomePageData.url_main" src="../assets/main.svg" class="web"/>
      </a>
      <a
        v-if="welcomePageData.url_facebook"
        :href="welcomePageData.url_facebook"
        target="_blank"
      >
      <img v-if="welcomePageData.url_facebook" src="../assets/facebook.svg" class="facebook"/>
      </a>
      <a
        v-if="welcomePageData.url_instagram"
        :href="welcomePageData.url_instagram"
        target="_blank"
      >
      <img v-if="welcomePageData.url_instagram" src="../assets/instagram.svg" class="instagram"/>
    
      </a>
      <a
        v-if="welcomePageData.url_twitter"
        :href="welcomePageData.url_twitter"
        target="_blank"
      >
      <img v-if="welcomePageData.url_twitter" src="../assets/twitter.svg" class="twitter"/>
      </a>
    </div>
    <div id="footer">
      <a
        v-if="welcomePageData.url_facebook"
        :href="welcomePageData.url_facebook"
        target="_blank"
        >Follow us on Facebook</a
      >
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      welcomePageData: [],
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
  font-weight: 700;
  line-height: 14.63px;
  color: black;
  font-family: "Montserrat", sans-serif;
  padding: 10px 12px;
}
#footer a:hover {
  background-color: black;
  color: #ff7701;
}

#social img {
  display: inline;
}


</style>
