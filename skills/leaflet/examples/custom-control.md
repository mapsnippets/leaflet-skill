# Recipe: Extending Leaflet Controls (`L.Control.extend`) 🎛️✨

> Source: https://leafletjs.com/examples/extending/extending-3-controls.html

This guide demonstrates how to create a custom UI button control inside the map canvas, position it at any of the four corners, style it with CSS, attach event listeners, and prevent unwanted map click propagation.

---

## 1. HTML Setup & Styling

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Custom Leaflet Control</title>
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
  <style>
    body { margin: 0; padding: 0; }
    #map { width: 100vw; height: 100vh; }

    .custom-map-btn {
      background: #ffffff;
      border: 2px solid rgba(0, 0, 0, 0.2);
      border-radius: 6px;
      padding: 6px 12px;
      font-family: system-ui, sans-serif;
      font-size: 13px;
      font-weight: 600;
      color: #0084FF;
      cursor: pointer;
      box-shadow: 0 1px 5px rgba(0,0,0,0.3);
      display: flex;
      align-items: center;
      gap: 6px;
      transition: background 0.15s;
    }
    .custom-map-btn:hover { background: #f8fafc; }
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

const map = L.map('map').setView([50.0755, 14.4378], 13);

L.tileLayer(`https://api.maptiler.com/maps/streets-v4/{z}/{x}/{y}.png?key=${apiKey}`, {
  tileSize: 512, zoomOffset: -1, minZoom: 1, maxZoom: 19,
  attribution: '&copy; MapTiler'
}).addTo(map);

// 1. Extend L.Control to create custom UI element
L.Control.CenterPrague = L.Control.extend({
  options: {
    position: 'topright' // 'topleft', 'topright', 'bottomleft', 'bottomright'
  },

  onAdd: function (map) {
    const container = L.DomUtil.create('div', 'leaflet-bar');
    const button = L.DomUtil.create('button', 'custom-map-btn', container);
    
    button.innerHTML = `
      <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="#0084FF" stroke-width="2.5">
        <circle cx="12" cy="12" r="10"></circle>
        <circle cx="12" cy="12" r="3"></circle>
      </svg>
      <span>Prague Center</span>
    `;

    // CRITICAL: Prevent map from dragging or zooming when clicking this button
    L.DomEvent.disableClickPropagation(container);
    L.DomEvent.disableScrollPropagation(container);

    // Attach click action
    L.DomEvent.on(button, 'click', (e) => {
      L.DomEvent.stop(e);
      map.flyTo([50.0755, 14.4378], 14, { duration: 1.5 });
    });

    return container;
  },

  onRemove: function (map) {
    // Cleanup event listeners if needed
  }
});

// 2. Factory method
L.control.centerPrague = function (opts) {
  return new L.Control.CenterPrague(opts);
};

// 3. Add to map
L.control.centerPrague({ position: 'topright' }).addTo(map);
```
