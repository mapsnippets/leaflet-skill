# Leaflet API Reference — Raster & Vector Layers 🗺️🎨

> Exhaustive technical reference for Leaflet's raster tile engines (`L.TileLayer`, `L.TileLayer.WMS`), media overlays (`L.ImageOverlay`, `L.VideoOverlay`, `L.SVGOverlay`), vector geometries (`L.Path`, `L.Polyline`, `L.Polygon`, `L.Circle`, `L.CircleMarker`), `L.GeoJSON`, and hardware renderers (`L.SVG` vs `L.Canvas`).

Maintained by **[MapSnippets](https://mapsnippets.org/)** — Open-source geospatial snippets, guides, and agent tools.

---

## 1. Raster Tile Layers (`L.TileLayer`)

The primary engine for streaming slippy map tiles using Web Mercator (EPSG:3857) grid schemes.

```javascript
import L from 'leaflet';

const tileLayer = L.tileLayer('https://api.maptiler.com/maps/streets-v4/{z}/{x}/{y}.png?key=YOUR_API_KEY', {
  tileSize: 512,                  // MapTiler 512px retina standard
  zoomOffset: -1,                 // Offsets tile zoom level to align with 256px scale
  minZoom: 0,                     // Minimum allowed zoom level
  maxZoom: 22,                    // Maximum allowed zoom level
  maxNativeZoom: 19,              // Highest zoom level where actual tiles exist
  subdomains: 'abc',              // Sharding subdomains
  crossOrigin: true,              // Required for canvas export and pixel reading
  detectRetina: false,            // False when using native 512px tiles to prevent double scaling
  attribution: '<a href="https://www.maptiler.com/copyright/" target="_blank">&copy; MapTiler</a>'
}).addTo(map);
```

### Constructor Options Reference

| Option | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `minZoom` | `Number` | `0` | Minimum zoom level where layer is requested. |
| `maxZoom` | `Number` | `18` | Maximum zoom level where layer is visible. |
| `maxNativeZoom` | `Number` | `undefined` | Tiles exist up to this zoom; higher zooms will scale existing tiles instead of requesting 404s. |
| `minNativeZoom` | `Number` | `undefined` | Tiles exist down to this zoom; lower zooms will scale down tiles. |
| `tileSize` | `Number \| Point` | `256` | Tile size in pixels (`512` for MapTiler high-DPI). |
| `subdomains` | `String \| Array` | `'abc'` | Subdomains for URL template replacement `{s}`. |
| `errorTileUrl` | `String` | `''` | Fallback image URL shown if a tile fails to load. |
| `zoomOffset` | `Number` | `0` | Number added to `{z}` in the URL template (`-1` for 512px tiles). |
| `tms` | `Boolean` | `false` | Inverts the Y tile coordinate (TMS scheme). |
| `zoomReverse` | `Boolean` | `false` | Inverts zoom level numbers. |
| `detectRetina` | `Boolean` | `false` | If true, requests double-resolution tiles on retina screens. |
| `crossOrigin` | `Boolean \| String` | `false` | Sets `crossorigin` attribute for CORS canvas export. |
| `opacity` | `Number` | `1.0` | Layer opacity from `0.0` to `1.0`. |
| `zIndex` | `Number` | `1` | Stacking order within its pane. |
| `updateWhenIdle` | `Boolean` | `false` | Only fetch new tiles after panning has completely stopped. |
| `keepBuffer` | `Number` | `2` | Number of tile rows/columns outside viewport retained in memory. |

---

## 2. Enterprise WMS Layers (`L.TileLayer.WMS`)

Extends `L.TileLayer` to communicate with OGC Web Map Service (WMS) servers (GeoServer, MapServer, QGIS Server).

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

### Key WMS Parameters

| Parameter | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `layers` | `String` | `''` | **(Required)** Comma-separated list of WMS layers to render. |
| `styles` | `String` | `''` | Comma-separated list of layer styles. |
| `format` | `String` | `'image/jpeg'` | MIME type (`'image/png'` for transparent overlays). |
| `transparent` | `Boolean` | `false` | Request transparent background (`true` for overlays). |
| `version` | `String` | `'1.1.1'` | WMS standard version (`'1.1.1'` uses SRS, `'1.3.0'` uses CRS). |
| `uppercase` | `Boolean` | `false` | Forces WMS query parameter keys to uppercase (`LAYERS`, `BBOX`). |

---

## 3. Media & Image Overlays

### A. Georeferenced Orthophoto / Floorplan (`L.ImageOverlay`)

Pin a static raster image to geographic bounding coordinates:

```javascript
const imageBounds = [
  [50.07, 14.41], // South-West [lat, lng]
  [50.09, 14.45]  // North-East [lat, lng]
];

const imageOverlay = L.imageOverlay('assets/site_orthophoto.jpg', imageBounds, {
  opacity: 0.85,
  alt: 'Drone Survey 2026',
  interactive: true,
  crossOrigin: true
}).addTo(map);

// Recalibrate bounds or opacity at runtime:
imageOverlay.setBounds([[50.072, 14.412], [50.092, 14.452]]);
imageOverlay.setOpacity(0.5);
```

### B. Video Overlay (`L.VideoOverlay`)

Streams looping video directly onto map coordinates (ideal for Doppler weather radar or traffic simulations):

```javascript
const videoBounds = [[40.712, -74.227], [40.774, -74.125]];
const videoOverlay = L.videoOverlay('https://example.com/radar_loop.mp4', videoBounds, {
  opacity: 0.7,
  autoplay: true,
  loop: true,
  muted: true
}).addTo(map);

// Access native HTMLVideoElement:
videoOverlay.getElement().playbackRate = 1.5;
```

---

## 4. Vector Geometries (`L.Path`)

All vector geometries inherit from the `L.Path` abstract base class.

### Common Vector Styling Options (`PathOptions`)

| Option | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `stroke` | `Boolean` | `true` | Whether to draw a border stroke. |
| `color` | `String` | `'#3388ff'` | Stroke color (hex, rgb, hsl, CSS color). |
| `weight` | `Number` | `3` | Stroke width in pixels. |
| `opacity` | `Number` | `1.0` | Stroke opacity (`0.0` to `1.0`). |
| `lineCap` | `String` | `'round'` | Shape at end of stroke (`'butt'`, `'round'`, `'square'`). |
| `lineJoin` | `String` | `'round'` | Shape at stroke corners (`'miter'`, `'round'`, `'bevel'`). |
| `dashArray` | `String` | `null` | Stroke dash pattern (e.g. `'6, 8'`). |
| `dashOffset` | `String` | `null` | Distance into dash pattern to start stroke. |
| `fill` | `Boolean` | `depends` | Whether to fill the interior of the path (`true` for Polygons/Circles). |
| `fillColor` | `String` | `*color*` | Interior fill color. Defaults to `color` option. |
| `fillOpacity` | `Number` | `0.2` | Interior fill opacity (`0.0` to `1.0`). |
| `fillRule` | `String` | `'evenodd'`| Fill rule determining interior (`'nonzero'`, `'evenodd'`). |
| `renderer` | `Renderer`| `map renderer`| Explicit SVG or Canvas renderer instance. |
| `className` | `String` | `''` | Custom CSS class name attached to the SVG path element. |

---

## 5. Vector Geometry Classes

### A. `L.Polyline` (Lines, Tracks & Routes)
```javascript
const polyline = L.polyline([
  [50.0755, 14.4378],
  [50.0820, 14.4280],
  [50.0880, 14.4200]
], {
  color: '#0084FF',
  weight: 5,
  smoothFactor: 1.0 // Simplification tolerance for high-performance rendering
}).addTo(map);

// Add coordinate dynamically
polyline.addLatLng([50.0910, 14.4150]);
```

### B. `L.Polygon` (Polygons with Holes)
To construct polygons with interior holes (islands / rings), pass an array of linear rings where index `0` is the exterior ring and subsequent indices are holes:

```javascript
const exterior = [[50.07, 14.41], [50.09, 14.41], [50.09, 14.45], [50.07, 14.45]];
const hole = [[50.075, 14.42], [50.085, 14.42], [50.085, 14.44], [50.075, 14.44]];

const donutPolygon = L.polygon([exterior, hole], {
  color: '#ef4444',
  fillColor: '#fca5a5',
  fillOpacity: 0.5
}).addTo(map);
```

### C. `L.Circle` vs `L.CircleMarker`
* **`L.circle(latlng, { radius: 1000 })`** — Radius in **real-world meters**. Scales geographically as you zoom.
* **`L.circleMarker(latlng, { radius: 10 })`** — Radius in **screen pixels**. Fixed visual size across all zoom levels.

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

## 6. `L.GeoJSON` Factory & Options

`L.GeoJSON` parses GeoJSON FeatureCollections and applies data-driven styling, popups, and layer filtering:

```javascript
const geojsonLayer = L.geoJSON(data, {
  // 1. Convert Point geometries to circle markers or custom icons
  pointToLayer: (feature, latlng) => {
    return L.circleMarker(latlng, {
      radius: feature.properties.magnitude * 2.5,
      fillColor: feature.properties.magnitude > 5 ? '#ef4444' : '#0084ff',
      color: '#ffffff',
      weight: 1,
      fillOpacity: 0.8
    });
  },

  // 2. Dynamic polygon / polyline styling based on feature attributes
  style: (feature) => {
    return {
      fillColor: getColor(feature.properties.density),
      weight: 2,
      opacity: 1,
      color: 'white',
      dashArray: '3',
      fillOpacity: 0.7
    };
  },

  // 3. Attach event listeners and popups to each feature
  onEachFeature: (feature, layer) => {
    layer.bindPopup(`<strong>${feature.properties.name}</strong><br>Value: ${feature.properties.value}`);
    layer.on({
      mouseover: (e) => e.target.setStyle({ weight: 4, color: '#00D2FF' }),
      mouseout: (e) => geojsonLayer.resetStyle(e.target)
    });
  },

  // 4. Spatial attribute filtering
  filter: (feature) => {
    return feature.properties.active === true;
  }
}).addTo(map);
```

---

## 7. Vector Renderers: SVG vs Canvas

| Feature | SVG Renderer (`L.svg()`) | Canvas Renderer (`L.canvas()`) |
| :--- | :--- | :--- |
| **DOM Representation** | Individual `<path>` / `<svg>` elements per layer. | Single `<canvas>` element for all vector layers. |
| **Max Recommended Features** | ~1,000 features. | ~20,000 features. |
| **CSS Styling** | Directly stylable with CSS (`.leaflet-interactive`). | Not stylable via external CSS rules. |
| **DOM Inspection** | Inspectable via Chrome DevTools DOM tree. | Pixels on canvas, cannot inspect elements. |
| **Hover / Hit Detection** | Native browser DOM mouse events. | Leaflet color-keyed pixel hit testing. |
| **Best Used For** | Rich interactive shapes, animations, low counts. | Massive datasets, dense point scatters, GPS tracks. |

```javascript
// Switching to Canvas renderer for 5,000 points:
const canvasRenderer = L.canvas({ padding: 0.5 });

for (let i = 0; i < 5000; i++) {
  L.circleMarker([lat, lng], {
    renderer: canvasRenderer,
    radius: 4,
    color: '#0084FF',
    fillOpacity: 0.7
  }).addTo(map);
}
```
