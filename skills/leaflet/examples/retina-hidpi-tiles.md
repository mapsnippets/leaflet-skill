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

    // MapTiler 512px default raster tiles with Retina @2x resolution (no /512/ path prefix!)
    L.tileLayer(`https://api.maptiler.com/maps/streets-v4/{z}/{x}/{y}@2x.png?key=${MAPTILER_KEY}`, {
      tileSize: 512,
      zoomOffset: -1,
      maxZoom: 19,
      attribution: '&copy; MapTiler &copy; OpenStreetMap'
    }).addTo(map);
  </script>
</body>
</html>
```

## Key API Features & Tile URL Invariants
- **512px Standard Resolution (Default):** MapTiler Cloud serves 512px tiles by default. Never use `/512/` in the URL path.
  - Standard 512px: `https://api.maptiler.com/maps/streets-v4/{z}/{x}/{y}.png?key=KEY`
  - High-DPI Retina 512px: `https://api.maptiler.com/maps/streets-v4/{z}/{x}/{y}@2x.png?key=KEY`
- **Legacy 256px Tiles:** Only legacy 256px tiles include a size prefix: `https://api.maptiler.com/maps/streets-v4/256/{z}/{x}/{y}.png?key=KEY` (or `@2x.png`).
- **Leaflet Coordinate Alignment:** Configure `tileSize: 512` and `zoomOffset: -1` in Leaflet options to keep zoom math aligned.
