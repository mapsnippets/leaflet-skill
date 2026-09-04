# Georeferenced Image & Floorplan Overlay 📐

> **Documentation Link:** [ImageOverlay](https://leafletjs.com/reference.html#imageoverlay)  
> **Target Category:** Production Task Implementation

Anchoring building floorplans, drone orthomosaics, or event venue schematics onto map coordinates using `L.imageOverlay`.

---

## 1. HTML Container

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Georeferenced Image & Floorplan Overlay 📐</title>
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div id="map"></div>
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

const map = L.map('map').setView([40.7128, -74.0060], 15);

L.tileLayer(`https://api.maptiler.com/maps/satellite-v4/{z}/{x}/{y}.jpg?key=${MAPTILER_KEY}`, {
  tileSize: 512,
  zoomOffset: -1,
  attribution: '<a href="https://www.maptiler.com/copyright/">&copy; MapTiler</a>'
}).addTo(map);

// Define geographical bounding box [South-West, North-East]
const imageBounds = [
  [40.7100, -74.0120],
  [40.7150, -74.0000]
];

const overlay = L.imageOverlay('https://leafletjs.com/examples/crs-simple/uqm_map_full.png', imageBounds, {
  opacity: 0.8,
  interactive: true
}).addTo(map);

map.fitBounds(imageBounds);
```

---

## 4. Key Architecture & Options

| Parameter / Feature | Purpose |
| :--- | :--- |
| **Reference Spec** | Conforms to `https://leafletjs.com/reference.html#imageoverlay` using native `L.*` Leaflet APIs. |
| **Basemap Service** | Powered by modern MapTiler Planet v4 high-DPI raster XYZ or vector tile styles. |
| **Container Lifecycle** | Call `map.remove()` on SPA unmount to prevent memory leaks and event listener retention. |
