# Top Leaflet Plugins Reference & Code Recipes 🔌🗺️

Source: https://leafletjs.com/plugins.html

This guide provides working recipes and best practices for the 10 most popular and reliable Leaflet community plugins.

---

## 1. Heatmaps (`leaflet.heat`)

Fast, HTML5 Canvas-based heatmap layer for thousands of point densities.

### Installation:
```bash
npm install leaflet.heat
```

### Implementation:
```javascript
import L from "leaflet";
import "leaflet.heat";

// Points in format: [lat, lng, intensity (0.0 to 1.0)]
const heatData = [
  [50.0755, 14.4378, 0.9],
  [50.0800, 14.4250, 0.6],
  [50.0710, 14.4400, 0.4]
];

const heatLayer = L.heatLayer(heatData, {
  radius: 25,
  blur: 15,
  maxZoom: 17,
  max: 1.0,
  gradient: {
    0.2: "#0000ff",
    0.4: "#00ffff",
    0.6: "#00ff00",
    0.8: "#ffff00",
    1.0: "#ff0000"
  }
}).addTo(map);

// Dynamically add new points
heatLayer.addLatLng([50.078, 14.432, 0.8]);
```

---

## 2. Drawing & Editing Tools (`@geoman-io/leaflet-geoman-free`)

The modern standard for vector geometry drawing, editing, dragging, and snapping (superior successor to legacy `Leaflet.draw`).

### Installation:
```bash
npm install @geoman-io/leaflet-geoman-free
```

### Implementation:
```javascript
import L from "leaflet";
import "@geoman-io/leaflet-geoman-free";
import "@geoman-io/leaflet-geoman-free/dist/leaflet-geoman.css";

// Add Geoman drawing controls toolbar
map.pm.addControls({
  position: "topleft",
  drawMarker: true,
  drawCircleMarker: true,
  drawPolyline: true,
  drawRectangle: true,
  drawPolygon: true,
  drawCircle: true,
  editMode: true,
  dragMode: true,
  cutPolygon: true,
  removalMode: true
});

// Listen to shape creation
map.on("pm:create", (e) => {
  const layer = e.layer;
  const geojson = layer.toGeoJSON();
  console.log("Created shape:", geojson);

  // Bind popup to drawn shape
  layer.bindPopup("Custom drawn feature").openPopup();
});
```

---

## 3. Swipe / Before-After Comparison (`leaflet-side-by-side`)

Provides an interactive horizontal split slider to visually compare two tile layers (e.g., historical vs modern imagery, streets vs satellite).

### Installation:
```bash
npm install leaflet-side-by-side
```

### Implementation:
```javascript
import L from "leaflet";
import "leaflet-side-by-side";

const streetLayer = L.tileLayer("https://api.maptiler.com/maps/streets-v4/{z}/{x}/{y}.png?key=KEY", {
  tileSize: 512, zoomOffset: -1
}).addTo(map);

const satelliteLayer = L.tileLayer("https://api.maptiler.com/maps/satellite-v4/{z}/{x}/{y}.jpg?key=KEY", {
  tileSize: 512, zoomOffset: -1
}).addTo(map);

// Add split-screen slider
L.control.sideBySide(streetLayer, satelliteLayer).addTo(map);
```

---

## 4. Minimap / Inset Map (`leaflet-minimap`)

Renders a small overview map in the corner tracking the main map view.

### Installation:
```bash
npm install leaflet-minimap
```

### Implementation:
```javascript
import L from "leaflet";
import "leaflet-minimap";
import "leaflet-minimap/dist/Control.MiniMap.min.css";

const miniTileLayer = L.tileLayer("https://api.maptiler.com/maps/base-v4/{z}/{x}/{y}.png?key=KEY", {
  tileSize: 512, zoomOffset: -1, minZoom: 0, maxZoom: 13
});

new L.Control.MiniMap(miniTileLayer, {
  toggleDisplay: true,
  minimized: false,
  position: "bottomright",
  width: 150,
  height: 150,
  aimingRectOptions: { color: "#ff7800", weight: 1, interactive: false }
}).addTo(map);
```

---

## 5. Fullscreen Button (`leaflet.fullscreen`)

Native HTML5 fullscreen toggle control.

### Installation:
```bash
npm install leaflet.fullscreen
```

### Implementation:
```javascript
import L from "leaflet";
import "leaflet.fullscreen";
import "leaflet.fullscreen/Control.FullScreen.css";

const map = L.map("map", {
  fullscreenControl: true,
  fullscreenControlOptions: {
    position: "topleft"
  }
});
```

---

## 6. Multi-Format Spatial Loader (`leaflet-omnivore`)

Parses CSV, GPX, KML, WKT, and TopoJSON directly into Leaflet layers.

### Installation:
```bash
npm install leaflet-omnivore
```

### Implementation:
```javascript
import L from "leaflet";
import omnivore from "leaflet-omnivore";

// Load GPX hiking track
const gpxLayer = omnivore.gpx("hiking-trail.gpx")
  .on("ready", function() {
    map.fitBounds(gpxLayer.getBounds());
  })
  .addTo(map);

// Load CSV with lat/lon columns
const csvLayer = omnivore.csv("stores.csv", {
  latfield: "latitude",
  lonfield: "longitude",
  delimiter: ","
}).addTo(map);
```

---

## 7. Real-Time Tracking (`leaflet-realtime`)

Periodically fetches or streams real-time GPS locations (buses, flights, vehicles) and smoothly updates markers.

### Installation:
```bash
npm install leaflet-realtime
```

### Implementation:
```javascript
import L from "leaflet";
import "leaflet-realtime";

const realtime = L.realtime({
  url: "https://api.example.com/live-buses.json",
  crossOrigin: true,
  type: "json"
}, {
  interval: 3 * 1000, // Polling interval in ms
  getFeatureId: (featureData) => featureData.properties.vehicleId,
  pointToLayer: (feature, latlng) => {
    return L.marker(latlng, {
      icon: L.divIcon({
        className: "vehicle-marker",
        html: `<div style="background:#0084ff; color:white; border-radius:50%; width:20px; height:20px; text-align:center; font-size:11px; line-height:20px;">🚌</div>`
      })
    });
  }
}).addTo(map);
```

---

## 8. Routing & Turn-by-Turn (`leaflet-routing-machine`)

Calculates routes, driving times, and turn-by-turn navigation paths.

### Installation:
```bash
npm install leaflet-routing-machine
```

### Implementation:
```javascript
import L from "leaflet";
import "leaflet-routing-machine";
import "leaflet-routing-machine/dist/leaflet-routing-machine.css";

L.Routing.control({
  waypoints: [
    L.latLng(50.0755, 14.4378), // Start (Prague)
    L.latLng(49.1951, 16.6068)  // Destination (Brno)
  ],
  routeWhileDragging: true,
  lineOptions: {
    styles: [{ color: "#0084ff", opacity: 0.8, weight: 6 }]
  }
}).addTo(map);
```
