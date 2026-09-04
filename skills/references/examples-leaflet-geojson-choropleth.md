# Official Example: Interactive Choropleth Map 📊🗺️

> Source: https://leafletjs.com/examples/choropleth/

This case study demonstrates how to create a high-performance interactive choropleth map with GeoJSON: coloring states by numeric density attributes, implementing hover highlights, creating a custom HUD info box control, and adding a categorized color legend.

---

## 1. HTML Container & HUD Styles

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Leaflet Interactive Choropleth</title>
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
  <style>
    body { margin: 0; padding: 0; font-family: system-ui, -apple-system, sans-serif; }
    #map { width: 100vw; height: 100vh; }
    
    /* Top-Right Information HUD */
    .info {
      padding: 10px 14px;
      font-size: 14px;
      background: white;
      background: rgba(255, 255, 255, 0.95);
      box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
      border-radius: 6px;
      min-width: 180px;
    }
    .info h4 { margin: 0 0 6px; color: #0084FF; font-size: 16px; }

    /* Bottom-Right Legend Control */
    .legend {
      text-align: left;
      line-height: 20px;
      color: #334155;
      background: white;
      padding: 10px 14px;
      box-shadow: 0 0 15px rgba(0,0,0,0.2);
      border-radius: 6px;
    }
    .legend i {
      width: 18px;
      height: 18px;
      float: left;
      margin-right: 8px;
      opacity: 0.85;
      border-radius: 2px;
    }
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

const map = L.map('map').setView([37.8, -96], 4);

// Use MapTiler Dataviz Dark or Light for maximum choropleth contrast
L.tileLayer(`https://api.maptiler.com/maps/dataviz-v4-light/{z}/{x}/{y}.png?key=${apiKey}`, {
  tileSize: 512,
  zoomOffset: -1,
  minZoom: 2,
  maxZoom: 18,
  crossOrigin: true,
  attribution: '<a href="https://www.maptiler.com/copyright/" target="_blank">&copy; MapTiler</a>'
}).addTo(map);

// 1. Color scale function based on density attribute
function getColor(d) {
  return d > 1000 ? '#800026' :
         d > 500  ? '#BD0026' :
         d > 200  ? '#E31A1C' :
         d > 100  ? '#FC4E2A' :
         d > 50   ? '#FD8D3C' :
         d > 20   ? '#FEB24C' :
         d > 10   ? '#FED976' :
                    '#FFEDA0';
}

function style(feature) {
  return {
    fillColor: getColor(feature.properties.density),
    weight: 1.5,
    opacity: 1,
    color: '#ffffff',
    dashArray: '3',
    fillOpacity: 0.75
  };
}

// 2. Custom Info HUD Control
const info = L.control({ position: 'topright' });

info.onAdd = function (map) {
  this._div = L.DomUtil.create('div', 'info');
  this.update();
  return this._div;
};

info.update = function (props) {
  this._div.innerHTML = '<h4>US Population Density</h4>' + (props ?
    `<b>${props.name}</b><br />${props.density} people / mi²` :
    'Hover over a state');
};
info.addTo(map);

// 3. Interaction Handlers
let geojsonLayer;

function highlightFeature(e) {
  const layer = e.target;
  layer.setStyle({
    weight: 3,
    color: '#0084FF',
    dashArray: '',
    fillOpacity: 0.9
  });
  layer.bringToFront();
  info.update(layer.feature.properties);
}

function resetHighlight(e) {
  geojsonLayer.resetStyle(e.target);
  info.update();
}

function zoomToFeature(e) {
  map.fitBounds(e.target.getBounds());
}

function onEachFeature(feature, layer) {
  layer.on({
    mouseover: highlightFeature,
    mouseout: resetHighlight,
    click: zoomToFeature
  });
}

// 4. Ingest GeoJSON data (e.g. from US States dataset)
fetch('https://leafletjs.com/examples/choropleth/us-states.js')
  .then(() => {
    // Or fetch raw GeoJSON
    // geojsonLayer = L.geoJSON(statesData, { style, onEachFeature }).addTo(map);
  });

// 5. Custom Color Legend Control
const legend = L.control({ position: 'bottomright' });

legend.onAdd = function (map) {
  const div = L.DomUtil.create('div', 'info legend');
  const grades = [0, 10, 20, 50, 100, 200, 500, 1000];

  for (let i = 0; i < grades.length; i++) {
    div.innerHTML +=
      `<i style="background:${getColor(grades[i] + 1)}"></i> ` +
      `${grades[i]}${grades[i + 1] ? '&ndash;' + grades[i + 1] + '<br>' : '+'}`;
  }
  return div;
};
legend.addTo(map);
```
