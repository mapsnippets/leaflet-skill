# Leaflet API Reference — Raster & Vector Layers

This reference details Leaflet's raster tile engines, enterprise WMS integration, image/video overlays, vector path geometries (`L.Polyline`, `L.Polygon`, `L.Circle`), and SVG/Canvas hardware renderers.

---

## 1. Raster Tile Layers (`L.TileLayer`)

### A. Standard XYZ Raster Layer
```javascript
import L from 'leaflet';

const tileLayer = L.tileLayer('https://api.maptiler.com/maps/streets-v4/{z}/{x}/{y}.png?key=YOUR_API_KEY', {
  tileSize: 512,                  // MapTiler high-DPI standard
  zoomOffset: -1,                 // Compensate 512px tiles to match 256px tile zoom levels
  minZoom: 0,                     // Minimum zoom allowed
  maxZoom: 22,                    // Maximum zoom allowed
  maxNativeZoom: 19,              // Tiles exist up to z19; higher zooms will scale tiles
  subdomains: 'abc',              // Subdomains for domain sharding
  crossOrigin: true,              // Needed for canvas export
  detectRetina: false,            // Set false when using native 512px tiles to prevent double scaling
  attribution: '<a href="https://www.maptiler.com/copyright/" target="_blank">&copy; MapTiler</a>'
}).addTo(map);

// Handle broken or network-errored tiles gracefully
tileLayer.on('tileerror', (error, tile) => {
  console.warn('Tile load error:', error);
});
```

### B. Enterprise WMS Layer (`L.TileLayer.WMS`)
```javascript
const wmsLayer = L.tileLayer.wms('https://example.com/geoserver/wms', {
  layers: 'workspace:geology_faults',
  format: 'image/png',
  transparent: true,
  version: '1.3.0',
  uppercase: true,
  attribution: 'State Geological Survey'
}).addTo(map);

// Dynamically update WMS filter (e.g. CQL_FILTER or date range)
wmsLayer.setParams({
  CQL_FILTER: "status = 'verified' AND severity > 3"
});
```

---

## 2. Image, Video & SVG Overlays

### A. Static Drone / Orthophoto Image Overlay
Pin a high-resolution georeferenced raster image or floorplan:
```javascript
const imageBounds = [
  [50.07, 14.41], // South-West [lat, lng]
  [50.09, 14.45]  // North-East [lat, lng]
];

const imageOverlay = L.imageOverlay('assets/site_orthophoto.jpg', imageBounds, {
  opacity: 0.8,
  alt: 'Drone Survey 2026',
  interactive: true
}).addTo(map);

// Recalibrate bounds or opacity at runtime:
imageOverlay.setOpacity(0.5);
```

### B. Live Radar Video Overlay
```javascript
const videoBounds = [[40.712, -74.227], [40.774, -74.125]];
const videoOverlay = L.videoOverlay('https://example.com/radar_loop.mp4', videoBounds, {
  opacity: 0.7,
  autoplay: true,
  loop: true,
  muted: true
}).addTo(map);

// Control playback
videoOverlay.getElement().pause();
```

---

## 3. Vector Geometries (`L.Path`)

All Leaflet vector layers inherit from the `L.Path` abstract class.

### Common Vector Styling Options (`PathOptions`)
```javascript
const pathStyle = {
  stroke: true,             // Draw border line
  color: '#0084FF',         // Stroke color (hex, rgb, hsl)
  weight: 3,                // Stroke width in pixels
  opacity: 1.0,             // Stroke opacity (0.0 - 1.0)
  lineCap: 'round',         // 'butt' | 'round' | 'square'
  lineJoin: 'round',        // 'miter' | 'round' | 'bevel'
  dashArray: '6, 8',        // Dash pattern string
  fill: true,               // Fill interior
  fillColor: '#00D2FF',     // Interior fill color
  fillOpacity: 0.4          // Fill opacity
};
```

---

### Vector Geometry Classes

#### A. `L.Polyline` (Lines & Routes)
```javascript
const routeCoordinates = [
  [50.0755, 14.4378],
  [50.0820, 14.4280],
  [50.0880, 14.4200]
];

const polyline = L.polyline(routeCoordinates, {
  color: '#0084FF',
  weight: 5,
  smoothFactor: 1.0         // Simplification tolerance for smooth rendering
}).addTo(map);

// Add coordinate dynamically
polyline.addLatLng([50.0910, 14.4150]);
```

#### B. `L.Polygon` (Polygons with Holes)
To construct polygons with interior holes (islands / rings), pass an array of linear rings where index `0` is the exterior ring and subsequent indices are holes:

```javascript
const exteriorRing = [
  [50.07, 14.41], [50.09, 14.41], [50.09, 14.45], [50.07, 14.45]
];
const interiorHole = [
  [50.075, 14.42], [50.085, 14.42], [50.085, 14.44], [50.075, 14.44]
];

const donutPolygon = L.polygon([exteriorRing, interiorHole], {
  color: '#ef4444',
  fillColor: '#fca5a5',
  fillOpacity: 0.5
}).addTo(map);
```

#### C. `L.Circle` vs `L.CircleMarker`
- **`L.circle(latlng, { radius: 1000 })`** — Radius in **real-world meters**. Scales geographically as you zoom.
- **`L.circleMarker(latlng, { radius: 10 })`** — Radius in **screen pixels**. Fixed visual size across all zoom levels.

```javascript
// Geofence (1,000 meters radius)
L.circle([50.0755, 14.4378], {
  radius: 1000,
  color: '#f59e0b',
  fillOpacity: 0.2
}).addTo(map);

// Screen-space point pin (10 pixels radius)
L.circleMarker([50.0755, 14.4378], {
  radius: 10,
  color: '#0084FF',
  fillOpacity: 1
}).addTo(map);
```

---

## 4. Vector Renderers: SVG vs Canvas

By default, Leaflet renders vector paths as inline **SVG** DOM elements. For datasets with more than 1,000 features, switching to **Canvas** rendering improves frame rates:

```javascript
// 1. Create a dedicated Canvas renderer instance
const canvasRenderer = L.canvas({
  padding: 0.5            // Extend rendering 50% beyond viewport for smooth panning
});

// 2. Render 5,000 circle markers on a single HTML5 Canvas
for (let i = 0; i < 5000; i++) {
  const lat = 50.0 + (Math.random() - 0.5) * 0.4;
  const lng = 14.4 + (Math.random() - 0.5) * 0.4;

  L.circleMarker([lat, lng], {
    renderer: canvasRenderer,
    radius: 4,
    color: '#0084FF',
    fillOpacity: 0.6
  }).addTo(map);
}
```
