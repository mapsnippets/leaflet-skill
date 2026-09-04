# Official Example: Markers with Custom Icons 📍🎨

> Source: https://leafletjs.com/examples/custom-icons/

This guide demonstrates how to define customized marker icons using `L.Icon`, configure retina `@2x` resolutions, set anchor points and popup offsets, and implement an icon class factory pattern for multiple icon variants.

---

## 1. HTML Container & Asset Setup

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Leaflet Custom Icons</title>
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
  <style>
    body { margin: 0; padding: 0; }
    #map { width: 100vw; height: 100vh; }
    .icon-card { font-family: system-ui; font-size: 13px; }
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

const map = L.map('map').setView([51.5, -0.09], 13);

L.tileLayer(`https://api.maptiler.com/maps/streets-v4/{z}/{x}/{y}.png?key=${apiKey}`, {
  tileSize: 512,
  zoomOffset: -1,
  minZoom: 1,
  maxZoom: 19,
  crossOrigin: true,
  attribution: '<a href="https://www.maptiler.com/copyright/" target="_blank">&copy; MapTiler</a>'
}).addTo(map);

// 1. Define custom Leaflet Icon Class
const LeafIcon = L.Icon.extend({
  options: {
    shadowUrl: 'https://leafletjs.com/examples/custom-icons/leaf-shadow.png',
    iconSize: [38, 95],       // [width, height] in pixels
    shadowSize: [50, 64],
    iconAnchor: [22, 94],     // Point of the icon which corresponds to marker's location
    shadowAnchor: [4, 62],    // Point of the shadow corresponding to anchor
    popupAnchor: [-3, -76]    // Point from which the popup opens relative to iconAnchor
  }
});

// 2. Instantiate different colored variants
const greenIcon = new LeafIcon({ iconUrl: 'https://leafletjs.com/examples/custom-icons/leaf-green.png' });
const redIcon = new LeafIcon({ iconUrl: 'https://leafletjs.com/examples/custom-icons/leaf-red.png' });
const orangeIcon = new LeafIcon({ iconUrl: 'https://leafletjs.com/examples/custom-icons/leaf-orange.png' });

// 3. Add markers with custom popups
L.marker([51.497, -0.09], { icon: greenIcon })
  .addTo(map)
  .bindPopup('<div class="icon-card"><b>Eco Park Hub</b><br>Solar Powered Station</div>');

L.marker([51.495, -0.083], { icon: redIcon })
  .addTo(map)
  .bindPopup('<div class="icon-card"><b>Critical Emergency Node</b><br>High Priority</div>');

L.marker([51.505, -0.07], { icon: orangeIcon })
  .addTo(map)
  .bindPopup('<div class="icon-card"><b>Maintenance Facility</b><br>Active Inspections</div>');
```
