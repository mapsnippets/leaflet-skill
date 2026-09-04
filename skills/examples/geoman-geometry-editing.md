# Interactive Vector Drawing & Editing (Geoman) ✍️

> **Official Leaflet Reference:** [Leaflet Geoman Drawing Tools](https://github.com/geoman-io/leaflet-geoman)  
> **Target Category:** Production Task Implementation

Equip the map with an end-to-end vector digitizing suite allowing users to draw markers, polylines, polygons, edit vertices, cut holes, and rotate shapes.

---

## 1. HTML Container

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Interactive Vector Drawing & Editing (Geoman) ✍️</title>
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div id="map"></div>
<link rel="stylesheet" href="https://unpkg.com/@geoman-io/leaflet-geoman-free@latest/dist/leaflet-geoman.css" />
<script src="https://unpkg.com/@geoman-io/leaflet-geoman-free@latest/dist/leaflet-geoman.min.js"></script>
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

const map = L.map('map').setView([47.3769, 8.5417], 13); // Zurich

L.tileLayer(`https://api.maptiler.com/maps/outdoor-v4/{z}/{x}/{y}.png?key=${MAPTILER_KEY}`, {
  tileSize: 512,
  zoomOffset: -1,
  attribution: '<a href="https://www.maptiler.com/copyright/">&copy; MapTiler</a>'
}).addTo(map);

// Add Geoman toolbar controls
map.pm.addControls({
  position: 'topleft',
  drawCircle: true,
  drawMarker: true,
  drawPolygon: true,
  drawPolyline: true,
  editMode: true,
  dragMode: true,
  cutPolygon: true,
  removalMode: true
});

map.on('pm:create', (e) => {
  console.log('Created geometry:', e.layer.toGeoJSON());
});
```

---

## 4. Key Architecture & Options

| Parameter / Feature | Purpose |
| :--- | :--- |
| **Official Standard** | Conforms to `https://github.com/geoman-io/leaflet-geoman` using native `L.*` Leaflet APIs. |
| **Basemap Service** | Powered by modern MapTiler Planet v4 high-DPI raster XYZ or vector tile styles. |
| **Container Lifecycle** | Call `map.remove()` on SPA unmount to prevent memory leaks and event listener retention. |
