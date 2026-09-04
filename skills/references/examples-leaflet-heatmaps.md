# Official Example: Heatmap Density Layer (`leaflet.heat`) 🔥🗺️

> Source: https://github.com/Leaflet/Leaflet.heat

This guide demonstrates how to visualize thousands of geographic density points as a smooth, HTML5 Canvas-based heatmap surface using `leaflet.heat`.

---

## 1. HTML Setup

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Leaflet Heatmap</title>
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
  <style>
    body { margin: 0; padding: 0; }
    #map { width: 100vw; height: 100vh; }
  </style>
</head>
<body>
  <div id="map"></div>
  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
  <script src="https://unpkg.com/leaflet.heat@0.2.0/dist/leaflet-heat.js"></script>
  <script src="main.js"></script>
</body>
</html>
```

---

## 2. JavaScript Implementation

```javascript
import L from 'leaflet';
import 'leaflet/dist/leaflet.css';
import 'leaflet.heat';

const apiKey = 'YOUR_MAPTILER_API_KEY';

const map = L.map('map').setView([50.0755, 14.4378], 12);

// Dataviz Dark basemap creates an exceptional contrast backdrop for heatmaps
L.tileLayer(`https://api.maptiler.com/maps/dataviz-v4-dark/{z}/{x}/{y}.png?key=${apiKey}`, {
  tileSize: 512, zoomOffset: -1, minZoom: 2, maxZoom: 19,
  attribution: '&copy; MapTiler'
}).addTo(map);

// 1. Prepare points data in format: [lat, lng, intensity]
const heatPoints = [];
for (let i = 0; i < 3000; i++) {
  const lat = 50.0755 + (Math.random() - 0.5) * 0.15;
  const lng = 14.4378 + (Math.random() - 0.5) * 0.15;
  const intensity = Math.random(); // 0.0 to 1.0
  heatPoints.push([lat, lng, intensity]);
}

// 2. Initialize Heat Layer
const heatLayer = L.heatLayer(heatPoints, {
  radius: 25,     // Radius of each point on the heatmap in pixels
  blur: 18,       // Amount of blur applied
  maxZoom: 17,    // Zoom level where points reach maximum intensity
  max: 1.0,
  minOpacity: 0.2,
  gradient: {
    0.2: '#0084FF', // Cyan / Blue (Low)
    0.5: '#10b981', // Emerald (Medium)
    0.8: '#f59e0b', // Amber (High)
    1.0: '#ef4444'  // Red (Peak)
  }
}).addTo(map);

// 3. Dynamically append live incoming points
function addLiveHit(lat, lng, intensity) {
  heatLayer.addLatLng([lat, lng, intensity]);
}
```
