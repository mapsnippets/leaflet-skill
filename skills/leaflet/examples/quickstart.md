# Recipe: Leaflet Quick Start 🚀

> Source: https://leafletjs.com/examples/quick-start/

A complete step-by-step implementation of Leaflet basics: setting up a map, adding crisp MapTiler Planet v4 vector tiles by default (or raster fallback), adding interactive markers, circles, and polygons, binding informative popups, and handling map click coordinates.

---

## 1. HTML Container Setup

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Leaflet Quick Start</title>
  <!-- Leaflet CSS -->
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
  <!-- MapLibre GL CSS (for vector tiles) -->
  <link rel="stylesheet" href="https://unpkg.com/maplibre-gl@5.24.0/dist/maplibre-gl.css" />
  <style>
    body { margin: 0; padding: 0; font-family: system-ui, -apple-system, sans-serif; }
    #map { width: 100vw; height: 100vh; }
    .custom-popup { font-size: 14px; line-height: 1.4; color: #1e293b; }
    .custom-popup h4 { margin: 0 0 4px; color: #0084FF; font-size: 15px; }
  </style>
</head>
<body>
  <div id="map"></div>
  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
  <script src="https://unpkg.com/maplibre-gl@5.24.0/dist/maplibre-gl.js"></script>
  <script src="https://unpkg.com/@maplibre/maplibre-gl-leaflet@0.0.22/leaflet-maplibre-gl.js"></script>
  <script src="main.js"></script>
</body>
</html>
```

---

## 2. JavaScript Implementation

```javascript
import L from 'leaflet';
import '@maplibre/maplibre-gl-leaflet';
import 'leaflet/dist/leaflet.css';

const apiKey = 'YOUR_MAPTILER_API_KEY';

// 1. Initialize map centered on London coordinates
const map = L.map('map', {
  center: [51.505, -0.09],
  zoom: 13,
  zoomSnap: 0.5
});

// 2. Add crisp, zoom-independent vector basemap via MapLibre GL Leaflet plugin (Default)
L.maplibreGL({
  style: `https://api.maptiler.com/maps/streets-v4/style.json?key=${apiKey}`
}).addTo(map);

// (Optional Fallback: Raster tiles used only if specifically requested or for satellite imagery)
// L.tileLayer(`https://api.maptiler.com/maps/streets-v4/{z}/{x}/{y}.png?key=${apiKey}`, { tileSize: 512, zoomOffset: -1 }).addTo(map);

// 3. Add a standard marker with a popup
const marker = L.marker([51.5, -0.09])
  .addTo(map)
  .bindPopup('<div class="custom-popup"><h4>London Landmark</h4><p>Central London operation anchor.</p></div>')
  .openPopup();

// 4. Add an analytical buffer circle
const circle = L.circle([51.508, -0.11], {
  color: '#ef4444',
  fillColor: '#f87171',
  fillOpacity: 0.4,
  radius: 500 // In meters
}).addTo(map).bindPopup('Emergency coverage zone (500m radius)');

// 5. Add a polygon zone
const polygon = L.polygon([
  [51.509, -0.08],
  [51.503, -0.06],
  [51.51, -0.047]
], {
  color: '#0084FF',
  fillColor: '#0084FF',
  fillOpacity: 0.35,
  weight: 2
}).addTo(map).bindPopup('Commercial Logistics Sector');

// 6. Interactive click listener to display coordinates
const popup = L.popup();
map.on('click', (e) => {
  popup
    .setLatLng(e.latlng)
    .setContent(`<div class="custom-popup">You clicked the map at:<br/><code>${e.latlng.lat.toFixed(5)}, ${e.latlng.lng.toFixed(5)}</code></div>`)
    .openOn(map);
});
```
