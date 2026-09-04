# Rotating Marker with Dynamic Heading Bearing

> **Documentation Reference:** [Rotating Marker with Dynamic Heading Bearing](https://github.com/bbecquet/Leaflet.RotatedMarker)
> Category: **Markers, Popups & Custom Styling**

## Overview
Smoothly updates vehicle/aircraft marker heading angles based on movement direction using `leaflet-rotatedmarker`.

## Complete Standalone Example

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Leaflet - Rotated Marker Heading</title>
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/leaflet-rotatedmarker@0.2.0/leaflet.rotatedMarker.min.js"></script>
  <style>body { margin: 0; padding: 0; } #map { width: 100vw; height: 100vh; }</style>
</head>
<body>
  <div id="map"></div>
  <script>
    const MAPTILER_KEY = 'YOUR_MAPTILER_API_KEY';

    const map = L.map('map').setView([51.505, -0.09], 13);

    L.tileLayer(`https://api.maptiler.com/maps/streets-v4/{z}/{x}/{y}.png?key=${MAPTILER_KEY}`, {
      tileSize: 512, zoomOffset: -1, attribution: '&copy; MapTiler'
    }).addTo(map);

    const planeIcon = L.icon({
      iconUrl: 'https://cdn-icons-png.flaticon.com/512/7893/7893979.png',
      iconSize: [32, 32],
      iconAnchor: [16, 16]
    });

    const marker = L.marker([51.505, -0.09], {
      icon: planeIcon,
      rotationAngle: 45, // Heading in degrees (0 = North)
      rotationOrigin: 'center center'
    }).addTo(map);

    let angle = 45;
    setInterval(() => {
      angle = (angle + 5) % 360;
      marker.setRotationAngle(angle);
    }, 200);
  </script>
</body>
</html>
```

## Key API Features
- Native Leaflet API implementation.
- Modern MapTiler Planet v4 raster tiles.
- Clean and lightweight dependencies.
