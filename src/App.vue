<template>
  <div id="app">
    <Map></Map>
    <Sidebar></Sidebar>
  </div>
</template>

<script>
import Map from './components/Map.vue'
import Sidebar from './components/Sidebar.vue'
export default {
  name: 'App',
  components: {
    Map,
    Sidebar
  }
}
</script>

<style>

</style>
