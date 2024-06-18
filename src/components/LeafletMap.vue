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
      polylineCoordinates: [
        [23.4241, 53.8478],
        [23.5241, 53.9478],
      ],
      markerCoordinate: [23.5241, 53.9478],
      markerRotation: 45, // Rotation angle in degrees
    };
  },
  mounted() {
    this.initMap();
  },
  methods: {
    initMap() {
      this.map = L.map('map').setView([23.4241, 53.8478], 7);

      L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        maxZoom: 19,
      }).addTo(this.map);

      this.drawnItems = new L.FeatureGroup();
      this.map.addLayer(this.drawnItems);

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

      this.map.on(L.Draw.Event.CREATED, (event) => {
        const layer = event.layer;
        this.drawnItems.addLayer(layer);
      });

      this.addPolylineAndRotatedMarker();
    },
    addPolylineAndRotatedMarker() {
      // Add polyline
      const polyline = L.polyline(this.polylineCoordinates, { color: 'blue' }).addTo(this.map);

      // Add rotated marker
      const markerIcon = L.divIcon({
        className: 'rotated-marker',
        html: `<div style="transform: rotate(${this.markerRotation}deg);"><img src="https://leafletjs.com/examples/custom-icons/leaf-orange.png" style="width: 32px; height: 32px;" /></div>`,
        iconSize: [32, 32],
        iconAnchor: [16, 16],
      });

      const marker = L.marker(this.markerCoordinate, { icon: markerIcon }).addTo(this.map);
    },
  },
};
</script>

<style>
html, body, #app, #map-container, #map {
  height: 100%;
  margin: 0;
}

#map-container {
  height: 100vh; /* Full viewport height */
  display: flex;
  flex-direction: column;
}

#map {
  flex: 1; /* Take up remaining space */
}

.rotated-marker img {
  transform-origin: center;
}
</style>
