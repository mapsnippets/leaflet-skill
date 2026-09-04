# Official Example: Marker Clustering (`leaflet.markercluster`) 📍✨

> Source: https://github.com/Leaflet/Leaflet.markercluster

This guide explains how to cluster 10,000+ points smoothly using `leaflet.markercluster`, configure asynchronous non-blocking chunked loading, create customized glowing count bubbles, and handle spiderfy events.

---

## 1. HTML Container & Stylesheet Setup

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Leaflet Marker Clustering</title>
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
  <link rel="stylesheet" href="https://unpkg.com/leaflet.markercluster@1.5.3/dist/MarkerCluster.css" />
  <link rel="stylesheet" href="https://unpkg.com/leaflet.markercluster@1.5.3/dist/MarkerCluster.Default.css" />
  <style>
    body { margin: 0; padding: 0; }
    #map { width: 100vw; height: 100vh; }
  </style>
</head>
<body>
  <div id="map"></div>
  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
  <script src="https://unpkg.com/leaflet.markercluster@1.5.3/dist/leaflet.markercluster.js"></script>
  <script src="main.js"></script>
</body>
</html>
```

---

## 2. JavaScript Implementation

```javascript
import L from 'leaflet';
import 'leaflet/dist/leaflet.css';
import 'leaflet.markercluster/dist/MarkerCluster.css';
import 'leaflet.markercluster';

const apiKey = 'YOUR_MAPTILER_API_KEY';

const map = L.map('map').setView([50.0755, 14.4378], 12);

L.tileLayer(`https://api.maptiler.com/maps/streets-v4/{z}/{x}/{y}.png?key=${apiKey}`, {
  tileSize: 512, zoomOffset: -1, minZoom: 2, maxZoom: 19,
  attribution: '&copy; MapTiler'
}).addTo(map);

// 1. Configure High-Performance Cluster Group
const markers = L.markerClusterGroup({
  maxClusterRadius: 60,
  spiderfyOnMaxZoom: true,
  showCoverageOnHover: false,
  zoomToBoundsOnClick: true,
  disableClusteringAtZoom: 17,
  chunkedLoading: true,         // Prevents UI thread freeze with 10k+ points
  chunkInterval: 100,
  chunkDelay: 20,
  iconCreateFunction: function (cluster) {
    const count = cluster.getChildCount();
    let bg = '#0084FF'; // Blue (< 25)
    let size = 36;

    if (count > 200) {
      bg = '#ef4444'; // Red (> 200)
      size = 50;
    } else if (count > 50) {
      bg = '#f59e0b'; // Amber (> 50)
      size = 42;
    }

    return L.divIcon({
      html: `
        <div style="
          width: ${size}px; height: ${size}px;
          background: ${bg};
          border: 3px solid #ffffff;
          border-radius: 50%;
          box-shadow: 0 4px 10px rgba(0,0,0,0.3);
          color: white;
          font-family: system-ui;
          font-weight: bold;
          font-size: 13px;
          display: flex;
          align-items: center;
          justify-content: center;
        ">${count}</div>
      `,
      className: 'maptiler-cluster-icon',
      iconSize: L.point(size, size)
    });
  }
});

// 2. Generate 5,000 Points and Bulk Insert via addLayers
const markerList = [];
for (let i = 0; i < 5000; i++) {
  const lat = 50.0 + (Math.random() - 0.5) * 0.3;
  const lng = 14.4 + (Math.random() - 0.5) * 0.3;
  markerList.push(
    L.marker([lat, lng]).bindPopup(`<b>Station #${i}</b><br>Active`)
  );
}

// Bulk load array (much faster than individual addLayer)
markers.addLayers(markerList);
map.addLayer(markers);
```
