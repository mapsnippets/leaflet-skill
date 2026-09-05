---
name: leaflet
description: >-
  Expert coding skill for building fast, interactive web maps with Leaflet and MapTiler.
  Defaults to crisp WebGL vector basemaps via @maplibre/maplibre-gl-leaflet (L.maplibreGL) with
  MapTiler Planet v4 vector styles (streets-v4, outdoor-v4, dataviz-v4), using raster tiles (L.tileLayer)
  only when specifically requested or for satellite imagery. USE WHEN the user wants to create a map,
  add an interactive 2D map to a web or mobile app, display locations or routes, build a store locator,
  add markers, popups, or tooltips, render GeoJSON data on a map, create a choropleth map, build marker
  clustering, add a heatmap, handle map click or drag events, add drawing tools, integrate maps
  in React (react-leaflet), Next.js, Vue, or Svelte.
  Also USE WHEN the user mentions Leaflet, L.map, L.marker, L.tileLayer, L.maplibreGL, or Leaflet plugins.
license: MIT
metadata:
  author: mapsnippets
  homepage: https://mapsnippets.org/
---

# Leaflet — Agent Skill 🍃🗺️

> The authoritative AI coding standard for building fast, mobile-friendly, interactive web maps with **Leaflet** and modern vector/raster tile services.

Maintained by **[MapSnippets](https://mapsnippets.org/)** — Open-source geospatial snippets, guides, and agent tools.

---

## ⚡ Architectural Scope & Design Principles

* **VECTOR BASEMAPS BY DEFAULT (`L.maplibreGL`):** Every Leaflet map implementation **MUST default to vector basemaps** using `@maplibre/maplibre-gl-leaflet` (`L.maplibreGL`) with MapTiler Planet v4 vector styles (`https://api.maptiler.com/maps/{style}/style.json?key=YOUR_API_KEY`).
  - **Why Vector by Default:** Crisp, resolution-independent rendering at all screen DPIs, smooth fractional zooming without blurry text or pixelation, client-side dynamic restyling, and smaller network payloads.
  - All Leaflet vector overlays, markers, popups, tooltips, GeoJSON layers, polylines, choropleths, clustering, and plugins work natively on top of `L.maplibreGL`.
* **RASTER BASEMAPS ONLY WHEN SPECIFIC (`L.tileLayer`):** Use raster tiles (`L.tileLayer`) **ONLY IF** the user specifically and explicitly requests raster XYZ tiles (e.g. non-WebGL environments, legacy browser constraints), or when displaying satellite imagery (`satellite-v4`).
* **Architecture-First Reliability:** Leaflet pairs a simple DOM/SVG rendering engine with external plugins. Following strict lifecycle contracts and ecosystem boundaries eliminates crashes before they occur.

---

## 📐 Core Structural Design Contracts

### 1. Universal Map Lifecycle & Initialization Contract
Every Leaflet implementation must fulfill these four lifecycle rules:

```html
<!-- 1. Mandatory CSS Container Contract -->
<style>
  body { margin: 0; padding: 0; }
  #map { width: 100%; height: 100vh; position: relative; }
</style>
<div id="map"></div>

<!-- Leaflet Core -->
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<!-- Vector Basemap Bridge (Default Standard) -->
<link rel="stylesheet" href="https://unpkg.com/maplibre-gl@4.7.1/dist/maplibre-gl.css" />
<script src="https://unpkg.com/maplibre-gl@4.7.1/dist/maplibre-gl.js"></script>
<script src="https://unpkg.com/@maplibre/maplibre-gl-leaflet@0.0.22/leaflet-maplibre-gl.js"></script>

<script>
  // 2. Map Constructor Contract (Mandatory maxZoom: 19 + [lat, lng] Order)
  const map = L.map('map', {
    maxZoom: 19 // CRITICAL: Vector basemaps don't set maxZoom; without this, plugins crash with 'Map has no maxZoom specified'
  }).setView([50.0755, 14.4378], 13); // [latitude, longitude]

  // 3. Vector Basemap Addition
  L.maplibreGL({
    style: 'https://api.maptiler.com/maps/streets-v4/style.json?key=YOUR_API_KEY'
  }).addTo(map);

  // 4. Teardown Contract (For SPAs / React / Vue unmount)
  // map.remove(); // Removes DOM elements, clears event listeners
</script>
```

* ⚠️ **API Key Prompting Rule:** If the user does not supply an API key, use `YOUR_API_KEY` in code and include this prompt:
  > *"To display the map tiles, get a free MapTiler API key (100,000 monthly tile requests) at: https://docs.maptiler.com/cloud/api/authentication-key/"*
* ⚠️ **Coordinate Order Inversion Contract:** Leaflet APIs strictly require `[latitude, longitude]`. GeoJSON and standard spatial inputs provide `[longitude, latitude]`. Always swap explicitly when mapping: `[coord[1], coord[0]]`.

---

### 2. Raster Tile Fallback Contract (Satellite & Explicit Raster)
When the user explicitly requests raster tiles or displays satellite imagery (`satellite-v4`):

```javascript
L.tileLayer("https://api.maptiler.com/maps/satellite-v4/{z}/{x}/{y}.jpg?key=YOUR_API_KEY", {
  tileSize: 512,
  zoomOffset: -1,
  minZoom: 1,
  maxZoom: 19,
  attribution: '<a href="https://www.maptiler.com/copyright/" target="_blank">&copy; MapTiler</a> <a href="https://www.openstreetmap.org/copyright" target="_blank">&copy; OSM</a>',
  crossOrigin: true
}).addTo(map);
```
* ⚠️ **Tile URL Path Rule:** Never use `/512/` in the URL path — `.../maps/streets-v4/512/...` is **INVALID** on MapTiler Cloud. 512px tiles are native defaults and have no size prefix in their path.

---

### 3. Layer Panes & Stacking Hierarchy Contract
Leaflet organizes overlays into DOM panes. Manage z-index layering through explicit panes:

* **Pane Stacking Hierarchy:**
  1. `tilePane` (`z-index: 200`): Basemap tiles / WebGL canvas (`L.maplibreGL`).
  2. `overlayPane` (`z-index: 400`): Vector paths (`L.Polyline`, `L.Polygon`, `L.GeoJSON`).
  3. `shadowPane` (`z-index: 500`): Marker shadows.
  4. `markerPane` (`z-index: 600`): Pins, icons, custom HTML dots (`L.marker`, `L.divIcon`).
  5. `tooltipPane` (`z-index: 650`): Hover tooltips (`L.tooltip`).
  6. `popupPane` (`z-index: 700`): Active popups (`L.popup`).
* **Custom Labels Pane Pattern:** When creating custom panes for top-level labels or boundaries, always set `pointerEvents: 'none'` on the container so underlying markers remain clickable.

---

### 4. Ecosystem Capability & Plugin Boundary Matrix

| Capability | Architecture | Standard Implementation | Reference |
| :--- | :--- | :--- | :--- |
| **Vector Basemap** | **Bridge Plugin** | `@maplibre/maplibre-gl-leaflet` (`L.maplibreGL`) | [`references/plugins-catalog.md`](references/plugins-catalog.md) |
| **Vector Digitizing**| **Plugin Required**| `@geoman-io/leaflet-geoman-free` (sync to `L.FeatureGroup`) | [`examples/geoman-geometry-editing.md`](examples/geoman-geometry-editing.md) |
| **Clustering** | **Plugin Required**| `leaflet.markercluster` (**Requires `maxZoom: 19` on `L.map`**) | [`examples/marker-clustering.md`](examples/marker-clustering.md) |
| **Heatmap Density** | **Plugin Required**| `leaflet.heat` (Canvas overlay pane) | [`examples/heatmaps.md`](examples/heatmaps.md) |
| **Split Comparison** | **Plugin Required**| `leaflet-side-by-side` (⚠️ **Raster `L.tileLayer` only**, cannot clip canvas) | [`examples/side-by-side.md`](examples/side-by-side.md) |
| **Animated Paths** | **Plugin Required**| `leaflet-ant-path` (SVG marching ants) | [`examples/animated-polyline-ant-path.md`](examples/animated-polyline-ant-path.md) |
| **Minimap Inset** | **Plugin Required**| `leaflet-minimap` (Requires raster `L.tileLayer` in inset view) | [`examples/minimap-overview.md`](examples/minimap-overview.md) |
| **Fullscreen** | **Plugin Required**| `leaflet.fullscreen` (DOM control) | [`examples/fullscreen-toggle.md`](examples/fullscreen-toggle.md) |

---

## ⚡ Fast Search Topic Router

| Category | Location | Contents |
| :--- | :--- | :--- |
| **Task Examples** | **[examples/INDEX.md](examples/INDEX.md)** | **28 atomic runnable recipes** across Markers, Clusters, Choropleths, Panes, WMS, COG, and CAD |
| **Core API & Architecture** | **[references/INDEX.md](references/INDEX.md)** | Specifications for Map, UI Layers, Vector Layers, Controls, and Utilities |
| **Plugins Catalog** | **[references/plugins-catalog.md](references/plugins-catalog.md)** | Working recipes for Geoman, MarkerCluster, AntPath, Side-by-Side, Heatmaps |
| **Basemaps & Schemas** | `references/basemaps-*`, `references/vector-tile-*` | MapTiler Planet v4 tile URLs, 512px retina offsets, schemas, and REST endpoints |
| **Package Versions** | **[references/versions.md](references/versions.md)** | Pinned production releases for Leaflet (`v1.9.4`) and companion plugins |

---

## 🧪 Runnable Task Examples (`examples/`)

Browse **[examples/INDEX.md](examples/INDEX.md)** for the complete categorized catalog:

- [examples/quickstart.md](examples/quickstart.md) — Vector basemap with MapTiler Streets v4, scale bar, and interactive marker.
- [examples/marker-clustering.md](examples/marker-clustering.md) — 10,000+ points clustered with `leaflet.markercluster` and `maxZoom: 19`.
- [examples/geoman-geometry-editing.md](examples/geoman-geometry-editing.md) — Vector drawing, snapping, and editing toolbar with Geoman.
- [examples/geojson-choropleth.md](examples/geojson-choropleth.md) — Dynamic color classification, legend box, and hover highlight.
- [examples/side-by-side.md](examples/side-by-side.md) — Split-screen comparison slider with raster tile layers.
- [examples/animated-polyline-ant-path.md](examples/animated-polyline-ant-path.md) — Flowing dashed marching-ant transit line.
- [examples/custom-radar-pulse-marker.md](examples/custom-radar-pulse-marker.md) — Glowing animated pulse beacon using `L.divIcon`.
- [examples/map-panes.md](examples/map-panes.md) — Custom DOM pane z-index isolation and click-through handling.
- [examples/fullscreen-toggle.md](examples/fullscreen-toggle.md) — Fullscreen toggle control.
- [examples/minimap-overview.md](examples/minimap-overview.md) — Synchronized corner overview map.
- *...and 18 more task recipes in [examples/INDEX.md](examples/INDEX.md).*
