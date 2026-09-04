# Official Example: Swipe Comparison (`leaflet-side-by-side`) ↔️🗺️

> Source: https://github.com/digidem/leaflet-side-by-side

This tutorial shows how to build an interactive swipe comparison tool that splits the map canvas between satellite imagery on the left and a vector street map on the right.

---

## 1. HTML Setup

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Leaflet Side-by-Side Comparison</title>
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
  <style>
    body { margin: 0; padding: 0; }
    #map { width: 100vw; height: 100vh; }
  </style>
</head>
<body>
  <div id="map"></div>
  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
  <script src="https://unpkg.com/leaflet-side-by-side@2.2.0/leaflet-side-by-side.min.js"></script>
  <script src="main.js"></script>
</body>
</html>
```

---

## 2. JavaScript Implementation

```javascript
import L from 'leaflet';
import 'leaflet/dist/leaflet.css';
import 'leaflet-side-by-side';

const apiKey = 'YOUR_MAPTILER_API_KEY';

const map = L.map('map').setView([50.0755, 14.4378], 13);

// 1. Left Layer: High-Resolution Satellite v4
const satellite = L.tileLayer(`https://api.maptiler.com/maps/satellite-v4/{z}/{x}/{y}.jpg?key=${apiKey}`, {
  tileSize: 512, zoomOffset: -1, minZoom: 1, maxZoom: 19,
  attribution: '&copy; MapTiler'
}).addTo(map);

// 2. Right Layer: Modern Vector Streets v4
const streets = L.tileLayer(`https://api.maptiler.com/maps/streets-v4/{z}/{x}/{y}.png?key=${apiKey}`, {
  tileSize: 512, zoomOffset: -1, minZoom: 1, maxZoom: 19,
  attribution: '&copy; MapTiler &copy; OpenStreetMap'
}).addTo(map);

// 3. Attach Side-by-Side Comparison Slider Control
const sideBySide = L.control.sideBySide(satellite, streets).addTo(map);
```
