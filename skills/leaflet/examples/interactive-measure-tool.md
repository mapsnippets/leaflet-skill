# Interactive Measurement Tool (Distance & Area)

> **Documentation Reference:** [Interactive Measurement Tool (Distance & Area)](https://github.com/ljagis/leaflet-measure)
> Category: **Spatial Digitization, Heatmaps & Routing**

## Overview
Adds an interactive on-map ruler and polygon measurement tool calculating distances and land area using `leaflet-measure`.

## Complete Standalone Example

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Leaflet - Interactive Measure Tool</title>
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/leaflet-measure@3.1.0/dist/leaflet-measure.css">
  <script src="https://cdn.jsdelivr.net/npm/leaflet-measure@3.1.0/dist/leaflet-measure.min.js"></script>
  <style>body { margin: 0; padding: 0; } #map { width: 100vw; height: 100vh; }</style>
</head>
<body>
  <div id="map"></div>
  <script>
    const MAPTILER_KEY = 'YOUR_MAPTILER_API_KEY';

    const map = L.map('map').setView([48.8566, 2.3522], 12);

    L.tileLayer(`https://api.maptiler.com/maps/streets-v4/{z}/{x}/{y}.png?key=${MAPTILER_KEY}`, {
      tileSize: 512, zoomOffset: -1, attribution: '&copy; MapTiler'
    }).addTo(map);

    const measureControl = new L.Control.Measure({
      position: 'topright',
      primaryLengthUnit: 'kilometers',
      secondaryLengthUnit: 'meters',
      primaryAreaUnit: 'sqmeters',
      activeColor: '#0084FF',
      completedColor: '#00D2FF'
    });
    measureControl.addTo(map);
  </script>
</body>
</html>
```

## Key API Features
- Native Leaflet API implementation.
- Modern MapTiler Planet v4 raster tiles.
- Clean and lightweight dependencies.
