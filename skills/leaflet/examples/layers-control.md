# Recipe: Layer Groups & Layers Control 🎛️📑

> Source: https://leafletjs.com/examples/layers-control/

This tutorial shows how to organize markers and geometries into `L.layerGroup` containers and integrate `L.control.layers` to let users switch between mutually exclusive basemaps and toggleable overlay categories.

---

## 1. HTML Setup

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Leaflet Layer Groups & Controls</title>
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
  <style>
    body { margin: 0; padding: 0; }
    #map { width: 100vw; height: 100vh; }
  </style>
</head>
<body>
  <div id="map"></div>
  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
  <script src="main.js"></script>
</body>
</html>
```

---

## 2. JavaScript Implementation

```javascript
import L from 'leaflet';
import 'leaflet/dist/leaflet.css';

const apiKey = 'YOUR_MAPTILER_API_KEY';

// 1. Create Base Tile Layers (Mutually exclusive radio buttons)
const streets = L.tileLayer(`https://api.maptiler.com/maps/streets-v4/{z}/{x}/{y}.png?key=${apiKey}`, {
  tileSize: 512, zoomOffset: -1, minZoom: 1, maxZoom: 19,
  attribution: '&copy; MapTiler &copy; OpenStreetMap'
});

const outdoor = L.tileLayer(`https://api.maptiler.com/maps/outdoor-v4/{z}/{x}/{y}.png?key=${apiKey}`, {
  tileSize: 512, zoomOffset: -1, minZoom: 1, maxZoom: 19,
  attribution: '&copy; MapTiler &copy; OpenStreetMap'
});

const satellite = L.tileLayer(`https://api.maptiler.com/maps/satellite-v4/{z}/{x}/{y}.jpg?key=${apiKey}`, {
  tileSize: 512, zoomOffset: -1, minZoom: 1, maxZoom: 19,
  attribution: '&copy; MapTiler'
});

// 2. Create Overlay Markers & Layer Groups (Independent checkboxes)
const crownPoint = L.marker([39.73, -104.99]).bindPopup('Crown Point Station');
const rubyHill = L.marker([39.68, -105.00]).bindPopup('Ruby Hill Warehouse');
const goldenTriangle = L.marker([39.73, -104.98]).bindPopup('Golden Triangle Hub');

const cities = L.layerGroup([crownPoint, rubyHill, goldenTriangle]);

const shelterA = L.circle([39.70, -104.95], { radius: 1200, color: '#0084FF' }).bindPopup('Zone East');
const shelterB = L.circle([39.75, -105.02], { radius: 900, color: '#0084FF' }).bindPopup('Zone West');

const zones = L.layerGroup([shelterA, shelterB]);

// 3. Initialize Map with default layers
const map = L.map('map', {
  center: [39.73, -104.99],
  zoom: 11,
  layers: [streets, cities] // Active by default
});

// 4. Configure Layer Control
const baseMaps = {
  "Streets v4": streets,
  "Outdoor v4": outdoor,
  "Satellite v4": satellite
};

const overlayMaps = {
  "Stations": cities,
  "Emergency Zones": zones
};

L.control.layers(baseMaps, overlayMaps, {
  collapsed: false, // Keep open on desktop for quick toggling
  position: 'topright'
}).addTo(map);
```
