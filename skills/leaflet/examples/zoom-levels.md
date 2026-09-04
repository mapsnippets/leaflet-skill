# Configuring Fractional Zoom Levels & Snap 🔍

> **Documentation Link:** [Zoom levels](https://leafletjs.com/examples/zoom-levels/)  
> **Target Category:** Production Task Implementation

Fine-tuning camera zoom behavior in Leaflet using `zoomSnap`, `zoomDelta`, `wheelPxPerZoomLevel`, and smooth fractional zooming.

---

## 1. HTML Container

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Configuring Fractional Zoom Levels & Snap 🔍</title>
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div id="map"></div>
<div id="zoom-readout">Current Zoom: 12.5</div>
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

#zoom-readout { position: absolute; top: 16px; right: 16px; background: rgba(255,255,255,0.95); padding: 8px 14px; border-radius: 6px; box-shadow: 0 2px 8px rgba(0,0,0,0.15); font-size: 13px; font-weight: 600; z-index: 1000; }
```

---

## 3. Complete JavaScript Implementation

```javascript
const MAPTILER_KEY = 'YOUR_API_KEY';

const map = L.map('map', {
  zoomSnap: 0.25,   // Fractional zoom increments
  zoomDelta: 0.5,    // Zoom step per button click
  wheelPxPerZoomLevel: 120,
  minZoom: 2,
  maxZoom: 20
}).setView([14.4378, 50.0755], 12.5);

L.tileLayer(`https://api.maptiler.com/maps/streets-v4/{z}/{x}/{y}.png?key=${MAPTILER_KEY}`, {
  tileSize: 512,
  zoomOffset: -1,
  maxZoom: 22,
  attribution: '<a href="https://www.maptiler.com/copyright/">&copy; MapTiler</a>'
}).addTo(map);

const zoomReadout = document.getElementById('zoom-readout');
map.on('zoom', () => {
  zoomReadout.textContent = `Current Zoom: ${map.getZoom().toFixed(2)}`;
});
```

---

## 4. Key Architecture & Options

| Parameter / Feature | Purpose |
| :--- | :--- |
| **Reference Spec** | Conforms to `https://leafletjs.com/examples/zoom-levels/` using native `L.*` Leaflet APIs. |
| **Basemap Service** | Powered by modern MapTiler Planet v4 high-DPI raster XYZ or vector tile styles. |
| **Container Lifecycle** | Call `map.remove()` on SPA unmount to prevent memory leaks and event listener retention. |
