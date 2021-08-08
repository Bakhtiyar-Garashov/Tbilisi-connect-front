<template>
  <div id="app">
    <Map></Map>
    <Sidebar>

    </Sidebar>
    <WelcomePage>

    </WelcomePage>
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
      isWelcomePageActive: false,
    }
  },
  created(){
   
  },

  mounted(){
    
  }
}
</script>
<style>

</style>
