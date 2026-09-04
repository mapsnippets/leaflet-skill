---
name: leaflet
description: >-
  Expert coding skill for building lightweight, interactive web maps with Leaflet (v1.9+).
  USE WHEN the user wants to create a map, add an interactive 2D map to a web or mobile
  app, display locations or routes, build a store locator, add markers, popups, or
  tooltips, render GeoJSON data on a map, create a choropleth map, build marker
  clustering, add a heatmap, render vector tiles or raster tile layers, switch to
  satellite view, handle map click or drag events, add drawing tools, integrate maps
  in React (react-leaflet), Next.js, Vue, or Svelte, or build a lightweight mobile map.
  Also USE WHEN the user mentions Leaflet, L.map, L.marker, L.tileLayer, or Leaflet plugins.
license: MIT
metadata:
  author: mapsnippets
  homepage: https://mapsnippets.org/
---

# Leaflet — Agent Skill 🍃🗺️

> The authoritative AI coding standard for building fast, mobile-friendly, interactive web maps with **Leaflet (v1.9+)** and modern vector/raster tile services.

Maintained by **[MapSnippets](https://mapsnippets.org/)** — Open-source geospatial snippets, guides, and agent tools.

---

## ⚡ Architectural Scope & Data Reference Invariants

* **Native Library Focus:** This skill focuses strictly on pure, native **Leaflet** (`L.map`, `L.tileLayer`, `L.geoJSON`, `L.marker`, `L.divIcon`, `L.popup`, plugins like `leaflet.markercluster` and `@maplibre/maplibre-gl-leaflet`). All generated code must be 100% native Leaflet code.
* **MapTiler as Data Source:** MapTiler Cloud provides high-DPI raster XYZ tiles (512px with `zoomOffset: -1`), vector styles via `@maplibre/maplibre-gl-leaflet`, geocoding, and static maps.

---

## ⚡ Critical Invariants & Rules

### 🔑 Free Basemap API Key Prompting Invariant
* If the user does not provide an API key, use `YOUR_API_KEY` as the placeholder in generated code AND always include a friendly reminder guiding the user:
  > *"To display the map tiles, get a free MapTiler API key (100,000 monthly tile requests) at: https://docs.maptiler.com/cloud/api/authentication-key/"*

Follow these rules on every Leaflet code generation to prevent bugs:

### 1. ⚠️ Coordinate Order Inversion Rule
* **Leaflet APIs strictly expect `[latitude, longitude]`** (e.g., `L.map().setView([lat, lng], zoom)`, `L.marker([lat, lng])`, `L.latLng(lat, lng)`).
* **GeoJSON and standard geospatial data use `[longitude, latitude]`**.
* **Rule:** When crossing boundaries from GeoJSON, user inputs, or REST APIs into Leaflet methods, swap coordinates explicitly: `[coord[1], coord[0]]`.

### 2. 📐 Container Dimensions & `invalidateSize`
* Leaflet containers require an explicit CSS height (e.g., `height: 100vh; width: 100%`).
* If the map container resizes, is placed in a tab, modal, or accordion, or renders with missing tile gray boxes, call:
  ```javascript
  map.invalidateSize();
  ```

### 3. 🧹 Component Lifecycle & Memory Cleanup
* Always store the map instance and call `map.remove()` when unmounting or re-initializing in single-page apps (React, Vue, Svelte):
  ```javascript
  // React cleanup
  useEffect(() => {
    const map = L.map(containerRef.current).setView([50.0755, 14.4378], 13);
    // ...
    return () => map.remove();
  }, []);
  ```

### 4. 🎨 Vector Tiles in Leaflet
* For crisp, zoom-independent vector tiles in Leaflet, use `@maplibre/maplibre-gl-leaflet` (`L.maplibreGL`) with standard vector style JSON (`streets-v4`):
  ```javascript
  L.maplibreGL({
    style: "https://api.maptiler.com/maps/streets-v4/style.json?key=YOUR_API_KEY"
  }).addTo(map);
  ```

### 5. 🖼️ High-DPI Raster Tiles
* When using raster tile layers with 512px tiles, configure `tileSize: 512` and `zoomOffset: -1` to align zoom calculations:
  ```javascript
  L.tileLayer("https://api.maptiler.com/maps/streets-v4/{z}/{x}/{y}.png?key=YOUR_API_KEY", {
    tileSize: 512,
    zoomOffset: -1,
    minZoom: 1,
    attribution: '<a href="https://www.maptiler.com/copyright/" target="_blank">&copy; MapTiler</a> <a href="https://www.openstreetmap.org/copyright" target="_blank">&copy; OSM</a>',
    crossOrigin: true
  }).addTo(map);
  ```

---

## 🔎 Fast Search & Prefix Conventions

Target your `grep` or file searches in `references/` using these prefixes to quickly load exact task guides:

| Category | File Prefix | Contents |
| :--- | :--- | :--- |
| **Official Task Examples** | `examples-leaflet-*` | Atomic, copy-pasteable tutorials with full HTML, CSS, and native JS (Quickstart, Choropleth, Clusters, Custom Icons, Mobile, WMS, Panes, Side-by-Side) |
| **Core API References** | `api-*` | Authoritative specifications for Map, UI Layers, Vector Layers, Controls, and Utilities |
| **Basemaps & Services** | `basemaps-*`, `vector-tile-*`, `geocoding-*` | MapTiler Planet v4 tile endpoints, vector tile schemas, and REST services |

Consult **[references/INDEX.md](references/INDEX.md)** for the complete master index.

---

## 📚 Modular Reference Guides

Read these reference files on demand for deep implementation patterns:

- [references/INDEX.md](references/INDEX.md) — Master catalog of all API specs, practical guides, and official step-by-step examples.
- [references/examples-leaflet-quickstart.md](references/examples-leaflet-quickstart.md) — Official Quick Start guide with complete HTML, CSS, tiles, and popups.
- [references/examples-leaflet-geojson-choropleth.md](references/examples-leaflet-geojson-choropleth.md) — Interactive choropleth case study with custom info box and legend.
- [references/examples-leaflet-marker-clustering.md](references/examples-leaflet-marker-clustering.md) — Clustering 10k+ markers with custom count badges and spiderfy.
- [references/examples-leaflet-custom-icons.md](references/examples-leaflet-custom-icons.md) — Custom `L.Icon` classes, retina `@2x`, anchor offsets, and shadow alignment.
- [references/examples-leaflet-layers-control.md](references/examples-leaflet-layers-control.md) — Base layer radio buttons and overlay layer checkboxes.
- [references/examples-leaflet-mobile-geolocation.md](references/examples-leaflet-mobile-geolocation.md) — Mobile fullscreen viewport and GPS location tracking.
- [references/examples-leaflet-map-panes.md](references/examples-leaflet-map-panes.md) — Custom panes sandwich architecture (road labels over polygons).
- [references/examples-leaflet-side-by-side.md](references/examples-leaflet-side-by-side.md) — Swipe split comparison slider between satellite and street tiles.
- [references/vector-tiles-and-plugins.md](references/vector-tiles-and-plugins.md) — Vector tiles via `@maplibre/maplibre-gl-leaflet` & `Leaflet.VectorGrid`.
- [references/geojson-and-markers.md](references/geojson-and-markers.md) — `L.geoJSON`, `pointToLayer`, custom `L.divIcon`, `bindPopup`, `bindTooltip`, choropleths.
- [references/panes-and-zindex.md](references/panes-and-zindex.md) — Custom map panes (`map.createPane`) for strict layer and label z-index control.
- [references/frameworks.md](references/frameworks.md) — React (`react-leaflet` v4/v5 & vanilla hooks), Next.js SSR fix, Vue (`vue-leaflet`), Svelte.
- [references/patterns-gotchas.md](references/patterns-gotchas.md) — The 10 most common Leaflet bugs (bundler icon 404s, React double-init, missing CSS).
- [references/vector-tile-schemas.md](references/vector-tile-schemas.md) — Full 9-schema catalog (Planet v4, Outdoor SAC scales, Contours, 3D Buildings, Ocean, Cadastre).
- [references/basemaps-and-terrain.md](references/basemaps-and-terrain.md) — Map styles (`streets-v4`, `outdoor-v4`, `satellite-v4`, `dataviz-v4-dark`).
- [references/geocoding-and-services.md](references/geocoding-and-services.md) — Forward/reverse geocoding, search autocomplete, static maps, and elevation.

## 🗺️ Quickstart Patterns

### Option A: Vector Tiles in Leaflet (Recommended)

```javascript
import L from "leaflet";
import "@maplibre/maplibre-gl-leaflet";
import "leaflet/dist/leaflet.css";

const map = L.map("map", {
  center: [50.0755, 14.4378], // [lat, lng]
  zoom: 13
});

L.maplibreGL({
  style: "https://api.maptiler.com/maps/streets-v4/style.json?key=YOUR_API_KEY"
}).addTo(map);

// Add a marker with popup
const marker = L.marker([50.0755, 14.4378])
  .addTo(map)
  .bindPopup("<b>Prague</b><br>Czech Republic")
  .openPopup();
```

### Option B: High-DPI Raster Tiles

```javascript
import L from "leaflet";
import "leaflet/dist/leaflet.css";

const map = L.map("map").setView([50.0755, 14.4378], 13); // [lat, lng]

L.tileLayer("https://api.maptiler.com/maps/streets-v4/{z}/{x}/{y}.png?key=YOUR_API_KEY", {
  tileSize: 512,
  zoomOffset: -1,
  minZoom: 1,
  maxZoom: 20,
  attribution: '<a href="https://www.maptiler.com/copyright/" target="_blank">&copy; MapTiler</a> <a href="https://www.openstreetmap.org/copyright" target="_blank">&copy; OpenStreetMap contributors</a>',
  crossOrigin: true
}).addTo(map);
```
