<template>
  <cookie-consent>
    <template slot="message">
      <p>This website uses cookies</p>
    </template>
    <template slot="button">
      <button class="btn-accept">Accept</button>
    </template>
  </cookie-consent>
</template>

<script>
import CookieConsent from "vue-cookieconsent-component";

export default {
  components: {
    CookieConsent,
  },
};
</script>

<style lang="scss" scoped>
@import "./node_modules/vue-cookieconsent-component/src/scss/cookie-consent";
@import "./node_modules/vue-cookieconsent-component/src/scss/cookie-consent-bottom";
@import "./node_modules/vue-cookieconsent-component/src/scss/cookie-consent-transition";
</style>
