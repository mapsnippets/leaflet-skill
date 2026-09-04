# Recipe: WMS & TMS Integration 🌐🗺️

> Source: https://leafletjs.com/examples/wms/wms.html

This guide details how to integrate standard Open Geospatial Consortium (OGC) Web Map Services (WMS) in Leaflet using `L.tileLayer.wms`, handle transparent overlays, configure WMS versions, and inspect feature attributes.

---

## 1. HTML Setup

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Leaflet WMS Integration</title>
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
  <style>
    body { margin: 0; padding: 0; }
    #map { width: 100vw; height: 100vh; }
  </style>
</head>
<body>
  <div id="map"></div>
  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
  <script src="main.js"></script>
</body>
</html>
```

---

## 2. JavaScript Implementation

```javascript
import L from 'leaflet';
import 'leaflet/dist/leaflet.css';

const apiKey = 'YOUR_MAPTILER_API_KEY';

const map = L.map('map').setView([39.73, -104.99], 8);

// 1. Basemap Layer
const basemap = L.tileLayer(`https://api.maptiler.com/maps/streets-v4/{z}/{x}/{y}.png?key=${apiKey}`, {
  tileSize: 512,
  zoomOffset: -1,
  minZoom: 1,
  maxZoom: 19,
  attribution: '&copy; MapTiler'
}).addTo(map);

// 2. Transparent OGC WMS Layer Overlay (e.g. NOAA Weather Radar or USGS Topo)
const wmsRadar = L.tileLayer.wms('https://opengeo.ncep.noaa.gov/geoserver/conus/conus_bref_qpe/ows?', {
  layers: 'conus_bref_qpe',     // Target WMS layer name
  format: 'image/png',          // PNG ensures alpha transparency
  transparent: true,            // MANDATORY for overlaying on basemap
  version: '1.3.0',             // Use WMS 1.3.0 or 1.1.1
  opacity: 0.7,
  attribution: 'NOAA Weather Service'
}).addTo(map);

// 3. Dynamic WMS Parameter Updates
function changeRadarTime(timestampString) {
  wmsRadar.setParams({
    time: timestampString
  });
}
```
