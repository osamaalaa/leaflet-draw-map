<template>
  <div id="app">
    <leaflet-map ref="leafletMap"></leaflet-map>
  </div>
</template>

<script>
import LeafletMap from './components/LeafletMap.vue';
import 'leaflet/dist/leaflet.css';
import 'leaflet-draw/dist/leaflet.draw.css';
import 'leaflet-draw';

export default {
  name: 'App',
  components: {
    LeafletMap,
  },
};
</script>

<style>
#app {
  display: block !important;
  text-align: center;
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
