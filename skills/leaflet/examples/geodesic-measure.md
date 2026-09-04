# Geodesic Great-Circle Distance & Circles 🌐

> **Documentation Link:** [Leaflet.Geodesic](https://github.com/Fragger/Leaflet.Geodesic)  
> **Target Category:** Production Task Implementation

Draw true spherical great-circle navigation routes that curve accurately across the Mercator projection, avoiding planar distance distortion.

---

## 1. HTML Container

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Geodesic Great-Circle Distance & Circles 🌐</title>
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div id="map"></div>
<script src="https://cdn.jsdelivr.net/npm/leaflet.geodesic@2.7.1/dist/leaflet.geodesic.umd.min.js"></script>
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

const map = L.map('map').setView([30, 0], 2);

L.tileLayer(`https://api.maptiler.com/maps/streets-v4/{z}/{x}/{y}.png?key=${MAPTILER_KEY}`, {
  tileSize: 512,
  zoomOffset: -1,
  attribution: '<a href="https://www.maptiler.com/copyright/">&copy; MapTiler</a>'
}).addTo(map);

// True spherical great-circle line between London and Tokyo
const geodesicLine = new L.Geodesic([
  [51.5074, -0.1278], // London
  [35.6762, 139.6503] // Tokyo
], {
  weight: 4,
  opacity: 0.8,
  color: '#0084FF',
  steps: 50
}).addTo(map);
```

---

## 4. Key Architecture & Options

| Parameter / Feature | Purpose |
| :--- | :--- |
| **Reference Spec** | Conforms to `https://github.com/Fragger/Leaflet.Geodesic` using native `L.*` Leaflet APIs. |
| **Basemap Service** | Powered by modern MapTiler Planet v4 high-DPI raster XYZ or vector tile styles. |
| **Container Lifecycle** | Call `map.remove()` on SPA unmount to prevent memory leaks and event listener retention. |
