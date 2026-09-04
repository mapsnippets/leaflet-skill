# Recipe: Working with Map Panes (Sandwich Architecture) 🥪📑

> Source: https://leafletjs.com/examples/map-panes/

By default, Leaflet renders all vector paths (polygons, lines) in `overlayPane` (z-index 400), which places them *beneath* marker icons but *above* tile layers. This tutorial demonstrates how to create custom DOM panes (`map.createPane`) to implement the "sandwich" pattern: rendering basemap labels on top of custom vector choropleths while letting clicks pass through cleanly.

---

## 1. HTML Setup

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Leaflet Map Panes Sandwich</title>
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

const map = L.map('map').setView([50.0755, 14.4378], 13);

// 1. Create a custom pane for vector polygons (z-index 450)
map.createPane('polygonsPane');
map.getPane('polygonsPane').style.zIndex = 450;

// 2. Create a custom pane for basemap labels (z-index 550)
map.createPane('labelsPane');
map.getPane('labelsPane').style.zIndex = 550;

// CRITICAL: Let clicks and mouse events pass through the top label tiles down to the polygon
map.getPane('labelsPane').style.pointerEvents = 'none';

// 3. Bottom Layer: Basemap Without Labels (Base tilePane, z-index 200)
L.tileLayer(`https://api.maptiler.com/maps/base-v4/{z}/{x}/{y}.png?key=${apiKey}`, {
  tileSize: 512,
  zoomOffset: -1,
  minZoom: 1,
  maxZoom: 19,
  attribution: '&copy; MapTiler &copy; OpenStreetMap'
}).addTo(map);

// 4. Middle Layer: Custom GeoJSON Polygons (rendered in 'polygonsPane')
const zonePolygon = L.polygon([
  [50.08, 14.42],
  [50.07, 14.45],
  [50.06, 14.41]
], {
  pane: 'polygonsPane', // Target custom pane
  color: '#0084FF',
  fillColor: '#0084FF',
  fillOpacity: 0.65,
  weight: 2
}).addTo(map).bindPopup('<b>Historic Zone</b><br>Rendered beneath road labels!');

// 5. Top Layer: Road Labels & POIs Only (rendered in 'labelsPane' with pointerEvents none)
// Road labels appear crisply ABOVE the polygon fill, yet clicking the polygon still works!
```
