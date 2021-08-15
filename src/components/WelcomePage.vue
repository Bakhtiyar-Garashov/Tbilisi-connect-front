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
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="24"
          height="24"
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
          class="feather feather-globe"
        >
          <circle cx="12" cy="12" r="10"></circle>
          <line x1="2" y1="12" x2="22" y2="12"></line>
          <path
            d="M12 2a15.3 15.3 0 0 1 4 10 15.3 15.3 0 0 1-4 10 15.3 15.3 0 0 1-4-10 15.3 15.3 0 0 1 4-10z"
          ></path>
        </svg>
      </a>
      <a
        v-if="welcomePageData.url_facebook"
        :href="welcomePageData.url_facebook"
        target="_blank"
      >
        <svg
          width="24"
          height="24"
          viewBox="0 0 24 24"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            d="M12 21C16.9706 21 21 16.9706 21 12C21 7.02944 16.9706 3 12 3C7.02944 3 3 7.02944 3 12C3 16.9706 7.02944 21 12 21Z"
            stroke="black"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
          />
          <path
            d="M15.75 8.25H14.25C13.6533 8.25 13.081 8.48705 12.659 8.90901C12.2371 9.33097 12 9.90326 12 10.5V21"
            stroke="black"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
          />
          <path
            d="M9 13.5H15"
            stroke="black"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
          />
        </svg>
      </a>
      <a
        v-if="welcomePageData.url_instagram"
        :href="welcomePageData.url_instagram"
        target="_blank"
      >
        <svg
          width="24"
          height="24"
          viewBox="0 0 24 24"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            d="M12 15.75C14.0711 15.75 15.75 14.0711 15.75 12C15.75 9.92893 14.0711 8.25 12 8.25C9.92893 8.25 8.25 9.92893 8.25 12C8.25 14.0711 9.92893 15.75 12 15.75Z"
            stroke="black"
            stroke-width="2"
            stroke-miterlimit="10"
          />
          <path
            d="M16.125 3.375H7.875C5.38972 3.375 3.375 5.38972 3.375 7.875V16.125C3.375 18.6103 5.38972 20.625 7.875 20.625H16.125C18.6103 20.625 20.625 18.6103 20.625 16.125V7.875C20.625 5.38972 18.6103 3.375 16.125 3.375Z"
            stroke="black"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
          />
          <path
            d="M16.875 8.25C17.4963 8.25 18 7.74632 18 7.125C18 6.50368 17.4963 6 16.875 6C16.2537 6 15.75 6.50368 15.75 7.125C15.75 7.74632 16.2537 8.25 16.875 8.25Z"
            fill="black"
          />
        </svg>
      </a>
      <a
        v-if="welcomePageData.url_twitter"
        :href="welcomePageData.url_twitter"
        target="_blank"
      >
        <svg
          width="24"
          height="24"
          viewBox="0 0 24 24"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            d="M12.0005 8.25027C12.0006 7.38867 12.2973 6.55337 12.8408 5.8848C13.3843 5.21624 14.1414 4.75517 14.9848 4.57913C15.8282 4.40309 16.7065 4.5228 17.4721 4.91814C18.2376 5.31348 18.8437 5.96034 19.1885 6.74997L22.5005 6.75L19.4759 9.77461C19.279 12.8168 17.9316 15.67 15.7074 17.7548C13.4833 19.8397 10.549 20.9999 7.50051 21C4.50051 21 3.75051 19.875 3.75051 19.875C3.75051 19.875 6.75051 18.75 8.25051 16.5C8.25051 16.5 2.25051 13.5 3.75051 5.25C3.75051 5.25 7.50051 9 11.9992 9.75L12.0005 8.25027Z"
            stroke="black"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
          />
        </svg>
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
    <button id="hide" @click.prevent="emitHideCommand">Hide</button>
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
  methods:{
     emitHideCommand(){
          this.$emit('emitHideCommand');
      }
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
  padding: 10px 12px;
}
#footer a:hover {
  background-color: black;
  color: #ff7701;
}

#social svg {
  display: inline;
  margin-right: 11.51px;
  margin-top: 28px;
}

#social path:hover {
  fill: #ff7701;
}
#hide {
  right: 40px;
  font-weight: 500;
  margin-top: calc(10vh - 30px);
  margin-bottom: 20px;
  float: right;
}
</style>
