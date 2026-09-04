# Leaflet Plugin Ecosystem for MapTiler

Reference for essential Leaflet plugins commonly used with MapTiler maps.

---

## leaflet.markercluster

The most popular Leaflet plugin. Groups nearby markers into clusters that expand on click.

```html
<link rel="stylesheet" href="https://unpkg.com/leaflet.markercluster@1.5.3/dist/MarkerCluster.css" />
<link rel="stylesheet" href="https://unpkg.com/leaflet.markercluster@1.5.3/dist/MarkerCluster.Default.css" />
<script src="https://unpkg.com/leaflet.markercluster@1.5.3/dist/leaflet.markercluster.js"></script>
```

```bash
npm install leaflet.markercluster
```

```javascript
const markers = L.markerClusterGroup({
  maxClusterRadius: 50,          // Cluster radius in pixels
  spiderfyOnMaxZoom: true,       // Spread overlapping markers
  showCoverageOnHover: true,     // Show cluster bounds on hover
  zoomToBoundsOnClick: true,     // Zoom to cluster on click
  disableClusteringAtZoom: 18,   // Stop clustering at this zoom
  chunkedLoading: true,          // Load in chunks (better for 10k+ markers)
  chunkInterval: 200,
  chunkDelay: 50
});

// Add markers from GeoJSON
geojsonData.features.forEach(feature => {
  const [lng, lat] = feature.geometry.coordinates;
  markers.addLayer(
    L.marker([lat, lng]).bindPopup(feature.properties.name)
  );
});

map.addLayer(markers);

// Zoom to all markers
map.fitBounds(markers.getBounds());
```

### Custom Cluster Icon

```javascript
const markers = L.markerClusterGroup({
  iconCreateFunction: (cluster) => {
    const count = cluster.getChildCount();
    const size = count < 100 ? 'small' : count < 1000 ? 'medium' : 'large';
    return L.divIcon({
      html: `<div><span>${count}</span></div>`,
      className: `marker-cluster marker-cluster-${size}`,
      iconSize: L.point(40, 40)
    });
  }
});
```

**Docs:** https://github.com/Leaflet/Leaflet.markercluster

---

## leaflet.heat

Simple, fast heatmap layer using HTML5 Canvas.

```html
<script src="https://unpkg.com/leaflet.heat@0.2.0/dist/leaflet-heat.js"></script>
```

```bash
npm install leaflet.heat
```

```javascript
// Data format: [[lat, lng, intensity], ...]
const heatData = [
  [50.08, 14.42, 0.8],
  [50.07, 14.43, 0.6],
  [50.09, 14.41, 1.0]
];

const heat = L.heatLayer(heatData, {
  radius: 25,        // Blur radius (px)
  blur: 15,          // Outer blur (px)
  maxZoom: 17,       // Stop increasing intensity at this zoom
  max: 1.0,          // Maximum point intensity
  minOpacity: 0.05,  // Minimum opacity
  gradient: {        // Color gradient
    0.4: 'blue',
    0.6: 'cyan',
    0.7: 'lime',
    0.8: 'yellow',
    1.0: 'red'
  }
}).addTo(map);

// Update data dynamically
heat.setLatLngs(newData);

// Add single point
heat.addLatLng([50.1, 14.4, 0.5]);
```

**Docs:** https://github.com/Leaflet/Leaflet.heat

---

## leaflet-draw

Drawing and editing tools for creating geometric shapes on the map.

```html
<link rel="stylesheet" href="https://unpkg.com/leaflet-draw@1.0.4/dist/leaflet.draw.css" />
<script src="https://unpkg.com/leaflet-draw@1.0.4/dist/leaflet.draw.js"></script>
```

```bash
npm install leaflet-draw
```

```javascript
const drawnItems = new L.FeatureGroup();
map.addLayer(drawnItems);

const drawControl = new L.Control.Draw({
  position: 'topright',
  draw: {
    polyline: {
      shapeOptions: { color: '#0066FF', weight: 3 }
    },
    polygon: {
      allowIntersection: false,
      shapeOptions: { color: '#FF0000' }
    },
    rectangle: true,
    circle: true,
    circlemarker: false,
    marker: true
  },
  edit: {
    featureGroup: drawnItems,
    remove: true
  }
});
map.addControl(drawControl);

// Handle created shapes
map.on(L.Draw.Event.CREATED, (e) => {
  const layer = e.layer;
  const type = e.layerType;

  drawnItems.addLayer(layer);

  // Get GeoJSON
  const geojson = layer.toGeoJSON();
  console.log(type, geojson);

  // Get area for polygons
  if (type === 'polygon' || type === 'rectangle') {
    const area = L.GeometryUtil.geodesicArea(layer.getLatLngs()[0]);
    console.log('Area:', area, 'm²');
  }
});

map.on(L.Draw.Event.EDITED, (e) => {
  e.layers.eachLayer((layer) => {
    console.log('Edited:', layer.toGeoJSON());
  });
});

map.on(L.Draw.Event.DELETED, (e) => {
  console.log('Deleted', e.layers.getLayers().length, 'shapes');
});
```

**Docs:** https://github.com/Leaflet/Leaflet.draw

---

## @maplibre/maplibre-gl-leaflet

Add MapLibre GL (vector tile) layers to a Leaflet map. Enables MapTiler vector styles in Leaflet.

```html
<script src="https://unpkg.com/maplibre-gl@4.7.1/dist/maplibre-gl.js"></script>
<link href="https://unpkg.com/maplibre-gl@4.7.1/dist/maplibre-gl.css" rel="stylesheet" />
<script src="https://unpkg.com/@maplibre/maplibre-gl-leaflet@0.0.22/leaflet-maplibre-gl.js"></script>
```

```javascript
// Add vector tile base layer
L.maplibreGL({
  style: 'https://api.maptiler.com/maps/streets-v4/style.json?key=YOUR_MAPTILER_KEY',
  attribution: '\u00a9 <a href="https://www.maptiler.com/">MapTiler</a>'
}).addTo(map);
```

> **Note:** This renders the entire map via MapLibre GL (WebGL). Leaflet markers/popups still work on top. Good for vector style quality with Leaflet's familiar API.

**Docs:** https://github.com/maplibre/maplibre-gl-leaflet

---

## @maptiler/leaflet-maptilersdk

Official MapTiler plugin for Leaflet vector tiles.

```bash
npm install @maptiler/leaflet-maptilersdk
```

```javascript
import L from 'leaflet';
import { MaptilerLayer } from '@maptiler/leaflet-maptilersdk';

const map = L.map('map').setView([50.1167, 14.4178], 12);
new MaptilerLayer({
  apiKey: 'YOUR_MAPTILER_KEY',
  style: 'streets-v4'  // or any MapTiler style name
}).addTo(map);
```

- Uses MapLibre GL JS under the hood
- Same style names as MapTiler Cloud
- Language auto-detection

**Docs:** https://docs.maptiler.com/leaflet/ · https://github.com/maptiler/maptiler-leaflet-maptilersdk

---

## leaflet-fullscreen

Fullscreen toggle control.

```html
<link rel="stylesheet" href="https://unpkg.com/leaflet-fullscreen@1.0.2/Control.FullScreen.css" />
<script src="https://unpkg.com/leaflet-fullscreen@1.0.2/Control.FullScreen.js"></script>
```

```javascript
map.addControl(new L.Control.Fullscreen({
  position: 'topleft'
}));

map.on('fullscreenchange', () => {
  console.log('Fullscreen:', map.isFullscreen());
});
```

---

## leaflet-minimap

Overview minimap control showing context.

```html
<link rel="stylesheet" href="https://unpkg.com/leaflet-minimap@3.6.1/dist/Control.MiniMap.min.css" />
<script src="https://unpkg.com/leaflet-minimap@3.6.1/dist/Control.MiniMap.min.js"></script>
```

```javascript
const miniMapLayer = L.tileLayer(
  'https://api.maptiler.com/maps/streets-v4/256/{z}/{x}/{y}.png?key=YOUR_MAPTILER_KEY',
  { attribution: '' }
);

new L.Control.MiniMap(miniMapLayer, {
  toggleDisplay: true,
  minimized: false,
  position: 'bottomright'
}).addTo(map);
```

---

## leaflet-search

Search control with autocomplete.

```html
<link rel="stylesheet" href="https://unpkg.com/leaflet-search@4.0.0/dist/leaflet-search.min.css" />
<script src="https://unpkg.com/leaflet-search@4.0.0/dist/leaflet-search.min.js"></script>
```

```javascript
// Search within existing GeoJSON layer
map.addControl(new L.Control.Search({
  layer: geojsonLayer,
  propertyName: 'name',
  marker: false,
  moveToLocation: (latlng, title, map) => {
    map.setView(latlng, 15);
  }
}));

// Or with MapTiler geocoding
map.addControl(new L.Control.Search({
  url: 'https://api.maptiler.com/geocoding/{s}.json?key=YOUR_MAPTILER_KEY&limit=5',
  jsonpParam: null,
  propertyName: 'place_name',
  propertyLoc: ['geometry', 'coordinates'],
  formatData: (data) => {
    const results = {};
    data.features.forEach(f => {
      results[f.place_name] = L.latLng(
        f.geometry.coordinates[1],
        f.geometry.coordinates[0]
      );
    });
    return results;
  }
}));
```

---

## leaflet-routing-machine

Turn-by-turn routing with directions panel.

```html
<link rel="stylesheet" href="https://unpkg.com/leaflet-routing-machine@3.2.12/dist/leaflet-routing-machine.css" />
<script src="https://unpkg.com/leaflet-routing-machine@3.2.12/dist/leaflet-routing-machine.js"></script>
```

```javascript
L.Routing.control({
  waypoints: [
    L.latLng(50.1167, 14.4178),  // Prague
    L.latLng(49.1951, 16.6068)   // Brno
  ],
  routeWhileDragging: true,
  showAlternatives: true
}).addTo(map);
```

> **Note:** The default routing engine is OSRM. For custom routing, you can integrate MapTiler or other routing APIs.

---

## Plugin Compatibility Notes

- All plugins listed here work with Leaflet 1.9.x
- When using multiple plugins, load Leaflet first, then plugins in order
- Some plugins may conflict — test combinations before deploying
- For NPM usage, check each plugin's README for import syntax
