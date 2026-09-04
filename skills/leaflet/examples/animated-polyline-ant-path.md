# Animated Marching Ants Polyline Route 🐜

> **Documentation Link:** [Leaflet Ant Path](https://github.com/rubenspgcavalcante/leaflet-ant-path)  
> **Target Category:** Production Task Implementation

Visualizing animated routes, flights, or transit lines with moving dashed marching ants effect.

---

## 1. HTML Container

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Animated Marching Ants Polyline Route 🐜</title>
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div id="map"></div>
<script src="https://cdn.jsdelivr.net/npm/leaflet-ant-path@1.3.0/dist/leaflet-ant-path.min.js"></script>
  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
  <script src="main.js"></script>
</body>
</html>
```

---

## 2. CSS Styling

```css
html, body {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
}

#map {
  width: 100%;
  height: 100%;
}

#map { width: 100%; height: 100%; }
```

---

## 3. Complete JavaScript Implementation

```javascript
const MAPTILER_KEY = 'YOUR_API_KEY';

const map = L.map('map').setView([48.8566, 2.3522], 6);

L.tileLayer(`https://api.maptiler.com/maps/dataviz-v4-dark/{z}/{x}/{y}.png?key=${MAPTILER_KEY}`, {
  tileSize: 512,
  zoomOffset: -1,
  attribution: '<a href="https://www.maptiler.com/copyright/">&copy; MapTiler</a>'
}).addTo(map);

const routeCoords = [
  [48.8566, 2.3522], // Paris
  [50.8503, 4.3517], // Brussels
  [52.3676, 4.9041], // Amsterdam
  [52.5200, 13.4050] // Berlin
];

// Leaflet AntPath polyline
const antPath = L.polyline.antPath(routeCoords, {
  delay: 400,
  dashArray: [10, 20],
  weight: 5,
  color: '#0084FF',
  pulseColor: '#FFFFFF',
  paused: false,
  reverse: false
}).addTo(map);

map.fitBounds(antPath.getBounds(), { padding: [40, 40] });
```

---

## 4. Key Architecture & Options

| Parameter / Feature | Purpose |
| :--- | :--- |
| **Reference Spec** | Conforms to `https://github.com/rubenspgcavalcante/leaflet-ant-path` using native `L.*` Leaflet APIs. |
| **Basemap Service** | Powered by modern MapTiler Planet v4 high-DPI raster XYZ or vector tile styles. |
| **Container Lifecycle** | Call `map.remove()` on SPA unmount to prevent memory leaks and event listener retention. |
