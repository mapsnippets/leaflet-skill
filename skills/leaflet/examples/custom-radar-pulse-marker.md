# Pulsating CSS Radar Beacon Marker 📡

> **Documentation Link:** [Custom CSS DivIcon Marker](https://leafletjs.com/reference.html#divicon)  
> **Target Category:** Production Task Implementation

Create an animated, glowing radar ping marker using `L.divIcon` and CSS keyframe animations for real-time tracking.

---

## 1. HTML Container

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Pulsating CSS Radar Beacon Marker 📡</title>
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
.radar-beacon {
  width: 14px;
  height: 14px;
  background: #0084FF;
  border-radius: 50%;
  border: 2px solid #fff;
  position: relative;
}
.radar-beacon::after {
  content: '';
  position: absolute;
  top: -8px;
  left: -8px;
  width: 26px;
  height: 26px;
  border-radius: 50%;
  border: 2px solid #0084FF;
  animation: radar-ping 1.5s infinite ease-out;
}
@keyframes radar-ping {
  0% { transform: scale(0.5); opacity: 1; }
  100% { transform: scale(2.2); opacity: 0; }
}
```

---

## 3. Complete JavaScript Implementation

```javascript
const MAPTILER_KEY = 'YOUR_API_KEY';

const map = L.map('map').setView([50.0755, 14.4378], 14);

L.tileLayer(`https://api.maptiler.com/maps/dataviz-v4-dark/{z}/{x}/{y}.png?key=${MAPTILER_KEY}`, {
  tileSize: 512,
  zoomOffset: -1,
  attribution: '<a href="https://www.maptiler.com/copyright/">&copy; MapTiler</a>'
}).addTo(map);

const beaconIcon = L.divIcon({
  className: 'radar-beacon',
  iconSize: [14, 14],
  iconAnchor: [7, 7]
});

L.marker([50.0755, 14.4378], { icon: beaconIcon }).addTo(map);
```

---

## 4. Key Architecture & Options

| Parameter / Feature | Purpose |
| :--- | :--- |
| **Reference Spec** | Conforms to `https://leafletjs.com/reference.html#divicon` using native `L.*` Leaflet APIs. |
| **Basemap Service** | Powered by modern MapTiler Planet v4 high-DPI raster XYZ or vector tile styles. |
| **Container Lifecycle** | Call `map.remove()` on SPA unmount to prevent memory leaks and event listener retention. |
