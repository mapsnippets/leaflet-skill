# HiDPI / Retina 512px Tile Configuration

> **Documentation Reference:** [HiDPI / Retina 512px Tile Configuration](https://leafletjs.com/reference.html#tilelayer-detectretina)
> Category: **Quickstart & Core Basemaps**

## Overview
Configures crisp high-resolution `@2x` Retina raster tiles in Leaflet with `detectRetina: true` and 512px tile sizes.

## Complete Standalone Example

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Leaflet - HiDPI Retina Tiles</title>
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
  <style>body { margin: 0; padding: 0; } #map { width: 100vw; height: 100vh; }</style>
</head>
<body>
  <div id="map"></div>
  <script>
    const MAPTILER_KEY = 'YOUR_MAPTILER_API_KEY';

    const map = L.map('map').setView([47.3769, 8.5417], 14); // Zurich

    L.tileLayer(`https://api.maptiler.com/maps/streets-v4/256/{z}/{x}/{y}@2x.png?key=${MAPTILER_KEY}`, {
      tileSize: 512,
      zoomOffset: -1,
      detectRetina: true,
      maxZoom: 19,
      attribution: '&copy; MapTiler &copy; OpenStreetMap'
    }).addTo(map);
  </script>
</body>
</html>
```

## Key API Features
- Native Leaflet API implementation.
- Modern MapTiler Planet v4 raster tiles.
- Clean and lightweight dependencies.
