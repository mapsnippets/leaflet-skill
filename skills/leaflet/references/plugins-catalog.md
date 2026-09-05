# Top Leaflet Plugins Reference & Integration Guide 🔌🗺️

> Authoritative catalog and technical implementation reference for top third-party plugins, controls, layer extensions, drawing suites, and vector bridges for **Leaflet v1.9.4**.

Maintained by **[MapSnippets](https://mapsnippets.org/)** — Open-source geospatial snippets, guides, and agent tools.

---

## 🔎 Quick Plugin Directory & Compatibility

| Category | Recommended Plugin | Package Name | Vector Basemap (`L.maplibreGL`) Compatible? | Recipe Link |
| :--- | :--- | :--- | :--- | :--- |
| **Vector Basemap Bridge**| MapLibre GL Leaflet | `@maplibre/maplibre-gl-leaflet` | **Native (Enables Vector Basemaps)** | [`quickstart.md`](../examples/quickstart.md) |
| **Drawing & Digitizing** | Leaflet Geoman | `@geoman-io/leaflet-geoman-free` | ✅ Yes (Renders on SVG overlay panes) | [`geoman-geometry-editing.md`](../examples/geoman-geometry-editing.md) |
| **Marker Clustering** | Leaflet MarkerCluster | `leaflet.markercluster` | ⚠️ **Yes, requires `maxZoom: 19` on `L.map`** | [`marker-clustering.md`](../examples/marker-clustering.md) |
| **Heatmaps** | Leaflet Heat | `leaflet.heat` | ✅ Yes (Renders on HTML5 Canvas overlay pane) | [`heatmaps.md`](../examples/heatmaps.md) |
| **Split-Screen Swipe** | Leaflet Side-by-Side | `leaflet-side-by-side` | ⚠️ **Raster tile layers only** (`L.tileLayer`) | [`side-by-side.md`](../examples/side-by-side.md) |
| **Animated Paths** | Leaflet Ant Path | `leaflet-ant-path` | ✅ Yes (SVG dash-offset animation overlay) | [`animated-polyline-ant-path.md`](../examples/animated-polyline-ant-path.md) |
| **Overview Minimap** | Leaflet MiniMap | `leaflet-minimap` | ✅ Yes (Requires raster `L.tileLayer` in inset) | [`minimap-overview.md`](../examples/minimap-overview.md) |
| **Fullscreen Toggle** | Leaflet Fullscreen | `leaflet.fullscreen` | ✅ Yes (Standard DOM control) | [`fullscreen-toggle.md`](../examples/fullscreen-toggle.md) |
| **Routing & Navigation** | Routing Machine | `leaflet-routing-machine` | ✅ Yes (OSRM routing over vector basemap) | — |
| **Format Parsers** | Leaflet Omnivore | `leaflet-omnivore` | ✅ Yes (Parses CSV, GPX, KML, WKT to GeoJSON) | [`gpx-track-viewer.md`](../examples/gpx-track-viewer.md) |

---

## 1. Vector Basemap Bridge (`@maplibre/maplibre-gl-leaflet`)

The core architectural plugin that powers high-resolution MapTiler Planet v4 vector styles (`streets-v4`, `outdoor-v4`, `dataviz-v4-dark`) directly inside Leaflet.

* **NPM:** `npm install leaflet maplibre-gl @maplibre/maplibre-gl-leaflet`
* **CDN:**
  ```html
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
  <link rel="stylesheet" href="https://unpkg.com/maplibre-gl@5.24.0/dist/maplibre-gl.css" />
  <script src="https://unpkg.com/maplibre-gl@5.24.0/dist/maplibre-gl.js"></script>
  <script src="https://unpkg.com/@maplibre/maplibre-gl-leaflet@0.0.22/leaflet-maplibre-gl.js"></script>
  ```
* **Integration Snippet:**
  ```javascript
  // CRITICAL: Always pass maxZoom: 19 when creating map to support plugins
  const map = L.map('map', { maxZoom: 19 }).setView([50.0755, 14.4378], 12);

  const glLayer = L.maplibreGL({
    style: `https://api.maptiler.com/maps/streets-v4/style.json?key=${KEY}`
  }).addTo(map);
  ```
* **Critical Invariants:**
  1. Always pass `{ maxZoom: 19 }` to `L.map`. Because `L.maplibreGL` is a canvas overlay rather than a raster tile grid, it does not set `map.options.maxZoom`. Without it, `map.getMaxZoom()` returns `Infinity`.
  2. To access the underlying MapLibre map instance, call `glLayer.getMaplibreMap()`.

---

## 2. Drawing & Editing Tools (`@geoman-io/leaflet-geoman-free`)

The modern standard for vector geometry drawing, editing, dragging, and snapping (superior successor to legacy `Leaflet.draw`).

* **Recipe Reference:** [`examples/geoman-geometry-editing.md`](../examples/geoman-geometry-editing.md)
* **NPM:** `npm install @geoman-io/leaflet-geoman-free`
* **CDN:**
  ```html
  <link rel="stylesheet" href="https://unpkg.com/@geoman-io/leaflet-geoman-free@latest/dist/leaflet-geoman.css" />
  <script src="https://unpkg.com/@geoman-io/leaflet-geoman-free@latest/dist/leaflet-geoman.min.js"></script>
  ```
* **Integration Snippet:**
  ```javascript
  const drawnItems = new L.FeatureGroup().addTo(map);

  map.pm.addControls({
    position: 'topleft',
    drawPolygon: true,
    drawPolyline: true,
    drawMarker: true,
    drawCircle: false,
    editMode: true,
    dragMode: true,
    cutPolygon: true,
    removalMode: true
  });

  map.on('pm:create', (e) => {
    drawnItems.addLayer(e.layer);
    console.log('Created GeoJSON:', e.layer.toGeoJSON());
  });
  ```
* **Best Practice:** Maintain an explicit `L.FeatureGroup` to collect drawn layers. You can extract the entire drawn dataset at any time via `drawnItems.toGeoJSON()`.

---

## 3. Marker Clustering (`leaflet.markercluster`)

Clusters thousands of coordinate markers into interactive numeric aggregate bubbles.

* **Recipe Reference:** [`examples/marker-clustering.md`](../examples/marker-clustering.md)
* **NPM:** `npm install leaflet.markercluster`
* **CDN:**
  ```html
  <link rel="stylesheet" href="https://unpkg.com/leaflet.markercluster@1.5.3/dist/MarkerCluster.css" />
  <link rel="stylesheet" href="https://unpkg.com/leaflet.markercluster@1.5.3/dist/MarkerCluster.Default.css" />
  <script src="https://unpkg.com/leaflet.markercluster@1.5.3/dist/leaflet.markercluster.js"></script>
  ```
* **Integration Snippet:**
  ```javascript
  const markers = L.markerClusterGroup({
    maxZoom: 19,               // Required to prevent 'Map has no maxZoom specified'
    maxClusterRadius: 50,
    spiderfyOnMaxZoom: true,
    chunkedLoading: true       // Prevents UI thread freeze with 10k+ points
  });

  stores.features.forEach(f => {
    const latlng = [f.geometry.coordinates[1], f.geometry.coordinates[0]]; // Swap to [lat, lng]
    markers.addLayer(L.marker(latlng).bindPopup(`<b>${f.properties.name}</b>`));
  });

  map.addLayer(markers);
  ```
* **Critical Gotcha (`Uncaught Map has no maxZoom specified`):**  
  When using vector basemap `L.maplibreGL`, `map.getMaxZoom()` returns `Infinity`. You **MUST** pass `maxZoom: 19` into `L.map('map', { maxZoom: 19 })` and `L.markerClusterGroup({ maxZoom: 19, ... })`.

---

## 4. Heatmaps (`leaflet.heat`)

Fast HTML5 Canvas-based heatmap layer for visualizing point density without individual DOM pins.

* **Recipe Reference:** [`examples/heatmaps.md`](../examples/heatmaps.md)
* **NPM:** `npm install leaflet.heat`
* **CDN:**
  ```html
  <script src="https://unpkg.com/leaflet.heat@0.2.0/dist/leaflet-heat.js"></script>
  ```
* **Integration Snippet:**
  ```javascript
  // Points: [lat, lng, intensity (0.0 to 1.0)]
  const heatPoints = [
    [50.0755, 14.4378, 0.9],
    [50.0800, 14.4250, 0.6],
    [50.0710, 14.4400, 0.4]
  ];

  const heat = L.heatLayer(heatPoints, {
    radius: 25,
    blur: 15,
    maxZoom: 17,
    max: 1.0,
    gradient: { 0.2: '#0084FF', 0.5: '#00D2FF', 0.8: '#f59e0b', 1.0: '#ef4444' }
  }).addTo(map);
  ```

---

## 5. Swipe / Before-After Comparison (`leaflet-side-by-side`)

Provides a draggable split-screen divider comparing two tile layers.

* **Recipe Reference:** [`examples/side-by-side.md`](../examples/side-by-side.md)
* **NPM:** `npm install leaflet-side-by-side`
* **CDN:**
  ```html
  <script src="https://unpkg.com/leaflet-side-by-side@2.2.0/leaflet-side-by-side.min.js"></script>
  ```
* **Integration Snippet:**
  ```javascript
  // Raster Tile Layers required for leaflet-side-by-side clipping
  const streetLayer = L.tileLayer(`https://api.maptiler.com/maps/streets-v4/{z}/{x}/{y}.png?key=${KEY}`, {
    tileSize: 512, zoomOffset: -1
  }).addTo(map);

  const satLayer = L.tileLayer(`https://api.maptiler.com/maps/satellite-v4/{z}/{x}/{y}.jpg?key=${KEY}`, {
    tileSize: 512, zoomOffset: -1
  }).addTo(map);

  L.control.sideBySide(streetLayer, satLayer).addTo(map);
  ```
* **Critical Invariant:** `leaflet-side-by-side` uses CSS `clip: rect(...)` on Leaflet's raster tile container panes (`leaflet-tile-pane`). It **CANNOT** clip a single WebGL canvas from `L.maplibreGL`. Always use raster `L.tileLayer` endpoints for this plugin.

---

## 6. Animated Polyline Flow (`leaflet-ant-path`)

Renders glowing, animated dashed lines ("marching ants") representing routes, transit tracks, or flow direction.

* **Recipe Reference:** [`examples/animated-polyline-ant-path.md`](../examples/animated-polyline-ant-path.md)
* **CDN:**
  ```html
  <script src="https://unpkg.com/leaflet-ant-path@1.3.0/dist/leaflet-ant-path.js"></script>
  ```
* **Integration Snippet:**
  ```javascript
  const routeCoords = [[50.08, 14.42], [50.09, 14.44], [50.10, 14.45]];

  const antPath = new L.Polyline.AntPath(routeCoords, {
    delay: 800,
    dashArray: [10, 20],
    weight: 4,
    color: '#0084FF',
    pulseColor: '#00D2FF',
    paused: false,
    reverse: false
  }).addTo(map);
  ```

---

## 7. Overview Minimap (`leaflet-minimap`)

Renders a synchronized inset overview map in the corner.

* **Recipe Reference:** [`examples/minimap-overview.md`](../examples/minimap-overview.md)
* **CDN:**
  ```html
  <link rel="stylesheet" href="https://unpkg.com/leaflet-minimap@3.6.1/dist/Control.MiniMap.min.css" />
  <script src="https://unpkg.com/leaflet-minimap@3.6.1/dist/Control.MiniMap.min.js"></script>
  ```
* **Integration Snippet:**
  ```javascript
  const miniTiles = L.tileLayer(`https://api.maptiler.com/maps/dataviz-v4-dark/{z}/{x}/{y}.png?key=${KEY}`, {
    tileSize: 512, zoomOffset: -1, minZoom: 0, maxZoom: 13
  });

  new L.Control.MiniMap(miniTiles, {
    position: 'bottomright',
    width: 160,
    height: 160,
    toggleDisplay: true,
    aimingRectOptions: { color: '#00D2FF', weight: 2 }
  }).addTo(map);
  ```

---

## 8. Fullscreen Control (`leaflet.fullscreen`)

Native HTML5 fullscreen toggle control.

* **Recipe Reference:** [`examples/fullscreen-toggle.md`](../examples/fullscreen-toggle.md)
* **CDN:**
  ```html
  <link rel="stylesheet" href="https://unpkg.com/leaflet.fullscreen@2.4.0/Control.FullScreen.css" />
  <script src="https://unpkg.com/leaflet.fullscreen@2.4.0/Control.FullScreen.js"></script>
  ```
* **Integration Snippet:**
  ```javascript
  const map = L.map('map', {
    fullscreenControl: true,
    fullscreenControlOptions: { position: 'topleft' }
  });
  ```

---

## 9. Routing & Turn-by-Turn (`leaflet-routing-machine`)

Calculates waypoints, driving paths, and direction instructions using OSRM.

* **CDN:**
  ```html
  <link rel="stylesheet" href="https://unpkg.com/leaflet-routing-machine@3.2.12/dist/leaflet-routing-machine.css" />
  <script src="https://unpkg.com/leaflet-routing-machine@3.2.12/dist/leaflet-routing-machine.js"></script>
  ```
* **Integration Snippet:**
  ```javascript
  L.Routing.control({
    waypoints: [
      L.latLng(48.8566, 2.3522), // Paris
      L.latLng(50.8503, 4.3517)  // Brussels
    ],
    routeWhileDragging: true,
    lineOptions: {
      styles: [{ color: '#0084FF', weight: 5, opacity: 0.8 }]
    }
  }).addTo(map);
  ```

---

## 10. Multi-Format Spatial Loader (`leaflet-omnivore`)

Loads GPX, KML, CSV, and WKT spatial files into Leaflet GeoJSON layers.

* **Recipe Reference:** [`examples/gpx-track-viewer.md`](../examples/gpx-track-viewer.md)
* **CDN:**
  ```html
  <script src="https://unpkg.com/leaflet-omnivore@0.3.4/leaflet-omnivore.min.js"></script>
  ```
* **Integration Snippet:**
  ```javascript
  const gpxLayer = omnivore.gpx('hiking-trail.gpx')
    .on('ready', function() {
      map.fitBounds(gpxLayer.getBounds());
    })
    .addTo(map);
  ```
