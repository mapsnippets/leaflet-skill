# Fullscreen Map Toggle Control ⛶

> **Official Leaflet Reference:** [Leaflet.fullscreen](https://github.com/Leaflet/Leaflet.fullscreen)  
> **Target Category:** Production Task Implementation

Provide a fullscreen toggle button conforming to standard browser Fullscreen API with automatic leaflet map size invalidation.

---

## 1. HTML Container

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Fullscreen Map Toggle Control ⛶</title>
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div id="map"></div>
<link rel="stylesheet" href="https://api.mapbox.com/mapbox.js/plugins/leaflet-fullscreen/v1.0.1/leaflet.fullscreen.css" />
<script src="https://api.mapbox.com/mapbox.js/plugins/leaflet-fullscreen/v1.0.1/Leaflet.fullscreen.min.js"></script>
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

const map = L.map('map', {
  fullscreenControl: true,
  fullscreenControlOptions: {
    position: 'topleft'
  }
}).setView([40.7128, -74.0060], 12); // NYC

L.tileLayer(`https://api.maptiler.com/maps/streets-v4/{z}/{x}/{y}.png?key=${MAPTILER_KEY}`, {
  tileSize: 512,
  zoomOffset: -1,
  attribution: '<a href="https://www.maptiler.com/copyright/">&copy; MapTiler</a>'
}).addTo(map);
```

---

## 4. Key Architecture & Options

| Parameter / Feature | Purpose |
| :--- | :--- |
| **Official Standard** | Conforms to `https://github.com/Leaflet/Leaflet.fullscreen` using native `L.*` Leaflet APIs. |
| **Basemap Service** | Powered by modern MapTiler Planet v4 high-DPI raster XYZ or vector tile styles. |
| **Container Lifecycle** | Call `map.remove()` on SPA unmount to prevent memory leaks and event listener retention. |
