# Official Example: Non-Geographical Maps (`L.CRS.Simple`) 🎮📐

> Source: https://leafletjs.com/examples/crs-simple/crs-simple.html

Leaflet can display indoor floor plans, video game maps, high-resolution scientific microscopy images, and astronomy star charts where coordinates are standard pixel `[y, x]` instead of latitude and longitude.

---

## 1. HTML Setup

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Non-Geographical Map</title>
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
  <style>
    body { margin: 0; padding: 0; background: #0b1118; }
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

// 1. Initialize Map with L.CRS.Simple (Flat Euclidean Grid)
const map = L.map('map', {
  crs: L.CRS.Simple,
  minZoom: -2,
  maxZoom: 2
});

// 2. Define Image Dimensions in Pixels: [height, width]
const w = 2048;
const h = 1536;
const url = '/assets/floorplan_level_1.png';

// In L.CRS.Simple, coordinates map as [y, x]
const southWest = map.unproject([0, h], map.getMaxZoom() - 1);
const northEast = map.unproject([w, 0], map.getMaxZoom() - 1);
const bounds = new L.LatLngBounds(southWest, northEast);

// 3. Add Image Overlay
L.imageOverlay(url, bounds).addTo(map);

// 4. Fit map to image bounds
map.fitBounds(bounds);
map.setMaxBounds(bounds);

// 5. Add Interactive Markers to Floorplan Rooms
const roomMarker = L.marker(map.unproject([1024, 768], map.getMaxZoom() - 1))
  .addTo(map)
  .bindPopup('<b>Conference Hall Alpha</b><br>Capacity: 120 people');
```
