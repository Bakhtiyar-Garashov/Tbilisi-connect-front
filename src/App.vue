<template>
  <div id="app">
    <Map></Map>
    <Sidebar>
      <template v-if="isWelcomePageActive" v-slot:welcome-page>  
         <WelcomePage></WelcomePage>
      </template>
    </Sidebar>
  </div>
</template>

<script>
import Map from './components/Map.vue'
import Sidebar from './components/Sidebar.vue'
import WelcomePage from './components/WelcomePage.vue'
export default {
  name: 'App',
  components: {
    Map,
    Sidebar,
    WelcomePage
  },
  data(){
    return {
      isWelcomePageActive: true,
    }
  }
}
</script>
<style>

</style>
