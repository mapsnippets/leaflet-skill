# Accessible Web Map (ARIA & Keyboard Navigation) ♿

> **Official Leaflet Reference:** [Accessible maps](https://leafletjs.com/examples/accessibility/)  
> **Target Category:** Production Task Implementation

Making web maps fully accessible to screen readers and keyboard users via ARIA attributes, tab indices, alt text, and semantic descriptions.

---

## 1. HTML Container

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Accessible Web Map (ARIA & Keyboard Navigation) ♿</title>
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div id="map" role="region" aria-label="Interactive map of Europe"></div>
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
.sr-only { position: absolute; width: 1px; height: 1px; padding: 0; margin: -1px; overflow: hidden; clip: rect(0,0,0,0); border: 0; }
```

---

## 3. Complete JavaScript Implementation

```javascript
const MAPTILER_KEY = 'YOUR_API_KEY';

const map = L.map('map', {
  keyboard: true,
  keyboardPanDelta: 80
}).setView([50.0755, 14.4378], 13);

L.tileLayer(`https://api.maptiler.com/maps/streets-v4/{z}/{x}/{y}.png?key=${MAPTILER_KEY}`, {
  tileSize: 512,
  zoomOffset: -1,
  attribution: '<a href="https://www.maptiler.com/copyright/">&copy; MapTiler</a>'
}).addTo(map);

// Add accessible marker with alt title and keyboard focusable popup
const marker = L.marker([50.0755, 14.4378], {
  title: 'Prague City Center marker',
  alt: 'Prague City Center landmark',
  riseOnHover: true
}).addTo(map);

marker.bindPopup('<h3>Prague</h3><p>Capital city of the Czech Republic.</p>', {
  autoClose: false,
  closeOnClick: false
});
```

---

## 4. Key Architecture & Options

| Parameter / Feature | Purpose |
| :--- | :--- |
| **Official Standard** | Conforms to `https://leafletjs.com/examples/accessibility/` using native `L.*` Leaflet APIs. |
| **Basemap Service** | Powered by modern MapTiler Planet v4 high-DPI raster XYZ or vector tile styles. |
| **Container Lifecycle** | Call `map.remove()` on SPA unmount to prevent memory leaks and event listener retention. |
