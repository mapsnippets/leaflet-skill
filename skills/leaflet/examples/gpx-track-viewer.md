# GPX Track & Elevation Profile Viewer

> Official Reference: [GPX Track & Elevation Profile Viewer](https://github.com/mpetazzoni/leaflet-gpx)
> Category: **Spatial Digitization, Heatmaps & Routing**

## Overview
Loads and displays GPS tracks, waypoints, and distance stats from `.gpx` files in Leaflet using the `leaflet-gpx` plugin.

## Complete Standalone Example

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Leaflet - GPX Track Viewer</title>
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/leaflet-gpx/1.7.0/gpx.min.js"></script>
  <style>
    body { margin: 0; padding: 0; font-family: sans-serif; }
    #map { width: 100vw; height: 100vh; }
    .gpx-info {
      position: absolute; top: 16px; left: 16px; z-index: 1000;
      background: rgba(15, 23, 42, 0.9); color: white; padding: 12px 18px;
      border-radius: 8px; font-size: 13px;
    }
  </style>
</head>
<body>
  <div id="map"></div>
  <div class="gpx-info">
    <strong>Track Stats</strong><br>
    Distance: <span id="dist">--</span> km<br>
    Elevation Gain: <span id="elev">--</span> m
  </div>
  <script>
    const MAPTILER_KEY = 'YOUR_MAPTILER_API_KEY';

    const map = L.map('map').setView([46.5, 8.0], 10);

    L.tileLayer(`https://api.maptiler.com/maps/outdoor-v4/{z}/{x}/{y}.png?key=${MAPTILER_KEY}`, {
      tileSize: 512, zoomOffset: -1, attribution: '&copy; MapTiler'
    }).addTo(map);

    const gpxUrl = 'https://raw.githubusercontent.com/mpetazzoni/leaflet-gpx/master/sample.gpx';

    new L.GPX(gpxUrl, {
      async: true,
      polyline_options: { color: '#0084FF', weight: 4, opacity: 0.85 }
    }).on('loaded', function (e) {
      map.fitBounds(e.target.getBounds());
      document.getElementById('dist').textContent = (e.target.get_distance() / 1000).toFixed(2);
      document.getElementById('elev').textContent = Math.round(e.target.get_elevation_gain());
    }).addTo(map);
  </script>
</body>
</html>
```

## Key API Features
- Native Leaflet API implementation.
- Modern MapTiler Planet v4 raster tiles.
- Clean and lightweight dependencies.
