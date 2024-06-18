<template>
  <div id="map-container">
    <div id="map"></div>
  </div>
</template>

<script>
import L from 'leaflet';
import 'leaflet/dist/leaflet.css';
import 'leaflet-draw/dist/leaflet.draw.css';
import 'leaflet-draw';

export default {
  name: 'LeafletMap',
  data() {
    return {
      map: null,
      drawnItems: null,
      drawControl: null,
    };
  },
  mounted() {
    this.initMap();
  },
  methods: {
    initMap() {
      console.log('Initializing map...');
      this.map = L.map('map').setView([23.4241, 53.8478], 7); // Centered at UAE
      console.log('Map initialized', this.map);

      L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        maxZoom: 19,
      }).addTo(this.map);

      console.log('Tile layer added');

      this.drawnItems = new L.FeatureGroup();
      this.map.addLayer(this.drawnItems);
      console.log('Feature group added');

      this.drawControl = new L.Control.Draw({
        edit: {
          featureGroup: this.drawnItems,
        },
        draw: {
          polygon: true,
          rectangle: true,
          circle: true,
          marker: false,
          polyline: false,
        },
      });
      this.map.addControl(this.drawControl);
      console.log('Draw control added');

      this.map.on(L.Draw.Event.CREATED, (event) => {
        const layer = event.layer;
        this.drawnItems.addLayer(layer);
        console.log('Shape drawn and added to feature group');
      });
    },
  },
};
</script>

<style>
#map-container {
  height: 100vh; /* Full viewport height */
  display: flex;
  flex-direction: column;
}

#map { 
  flex: 1; /* Take up remaining space */
}
</style>
