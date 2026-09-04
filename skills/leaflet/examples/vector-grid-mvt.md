# Client-Side MVT Vector Tiles with Leaflet.VectorGrid ⚡

> **Documentation Link:** [Leaflet.VectorGrid](https://github.com/Leaflet/Leaflet.VectorGrid)  
> **Target Category:** Production Task Implementation

Render binary Mapbox Vector Tiles (.pbf) directly in Leaflet using HTML5 Canvas rendering and custom layer styling without plugins that wrap MapLibre.

---

## 1. HTML Container

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Client-Side MVT Vector Tiles with Leaflet.VectorGrid ⚡</title>
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div id="map"></div>
<script src="https://unpkg.com/leaflet.vectorgrid@latest/dist/Leaflet.VectorGrid.bundled.js"></script>
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

const map = L.map('map').setView([50.0755, 14.4378], 13);

const vectorTileUrl = `https://api.maptiler.com/tiles/v4/{z}/{x}/{y}.pbf?key=${MAPTILER_KEY}`;

const vtLayer = L.vectorGrid.protobuf(vectorTileUrl, {
  rendererFactory: L.canvas.tile,
  vectorTileLayerStyles: {
    water: {
      fill: true,
      fillColor: '#0084FF',
      fillOpacity: 0.8,
      weight: 0
    },
    building: {
      fill: true,
      fillColor: '#cbd5e1',
      fillOpacity: 0.6,
      weight: 0.5,
      color: '#94a3b8'
    },
    transportation: {
      weight: 2,
      color: '#ffffff',
      opacity: 0.9
    }
  },
  maxNativeZoom: 14
}).addTo(map);
```

---

## 4. Key Architecture & Options

| Parameter / Feature | Purpose |
| :--- | :--- |
| **Reference Spec** | Conforms to `https://github.com/Leaflet/Leaflet.VectorGrid` using native `L.*` Leaflet APIs. |
| **Basemap Service** | Powered by modern MapTiler Planet v4 high-DPI raster XYZ or vector tile styles. |
| **Container Lifecycle** | Call `map.remove()` on SPA unmount to prevent memory leaks and event listener retention. |
