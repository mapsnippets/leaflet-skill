# Recipe: Leaflet on Mobile & Geolocation 📱📍

> Source: https://leafletjs.com/examples/mobile/

This tutorial demonstrates how to build a mobile-optimized fullscreen map, request user device GPS coordinates via `map.locate`, display animated location accuracy circles, and gracefully handle permission denials.

---

## 1. Mobile-Optimized HTML Viewport

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <!-- CRITICAL: Prevent unwanted pinch/zoom on mobile browser chrome -->
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
  <title>Leaflet Mobile Geolocation</title>
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
  <style>
    body {
      padding: 0;
      margin: 0;
    }
    html, body, #map {
      height: 100%;
      width: 100vw;
    }
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

// 1. Initialize Map without fixed initial center
const map = L.map('map');

L.tileLayer(`https://api.maptiler.com/maps/streets-v4/{z}/{x}/{y}.png?key=${apiKey}`, {
  tileSize: 512,
  zoomOffset: -1,
  minZoom: 2,
  maxZoom: 19,
  crossOrigin: true,
  attribution: '&copy; MapTiler &copy; OpenStreetMap'
}).addTo(map);

// 2. Request User Location with High Accuracy
map.locate({
  setView: true,              // Automatically centers and zooms map to user
  maxZoom: 16,
  enableHighAccuracy: true,
  timeout: 10000
});

// 3. Handle Successful Location Resolution
map.on('locationfound', (e) => {
  const radius = e.accuracy / 2;

  // Add position marker
  L.marker(e.latlng)
    .addTo(map)
    .bindPopup(`You are within <b>${radius.toFixed(0)} meters</b> of this point.`)
    .openPopup();

  // Add accuracy boundary circle
  L.circle(e.latlng, {
    radius: radius,
    color: '#0084FF',
    fillColor: '#0084FF',
    fillOpacity: 0.15,
    weight: 2
  }).addTo(map);
});

// 4. Handle Permission Denial or Timeout
map.on('locationerror', (e) => {
  console.warn('Geolocation failed:', e.message);
  // Fallback to default location
  map.setView([50.0755, 14.4378], 13);
  alert('Location access denied or unavailable. Centering on default city.');
});
```
