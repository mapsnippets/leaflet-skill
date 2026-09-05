# Leaflet Reference Index 🗂️⚡

> Master index and topic routing directory for all Leaflet agent references, API standards, comprehensive documentation guides, and MapTiler basemap integrations. Load on demand.

Maintained by **[MapSnippets](https://mapsnippets.org/)** — Open-source geospatial snippets, guides, and agent tools.

---

## 🔎 Fast Search & Directory Routing

| Topic Area | Directory / Prefix | Contents |
| :--- | :--- | :--- |
| **Official API Docs** | **[references/api-catalog.md](api-catalog.md)** | **Exhaustive catalog of all 55 official Leaflet classes & modules** linking to [`leafletjs.com/reference.html`](https://leafletjs.com/reference.html) |
| **Task Examples** | **[examples/INDEX.md](../examples/INDEX.md)** | **28 atomic runnable recipes** with full HTML, CSS, and native JS across Markers, Clusters, Choropleths, Panes, WMS, COG, and CAD |
| **Official Tutorials**| **[references/examples-catalog.md](examples-catalog.md)** | Full index of all official Leaflet tutorials cross-referenced to standalone recipes |
| **Core API & Map** | `references/api-map.md`, `references/api-layers-controls-base.md` | `L.Map` options, camera navigation, event models, container lifecycle |
| **Layers & Media** | `references/api-raster-and-vector-layers.md`, `references/geojson-*` | `TileLayer`, `TileLayer.WMS`, `ImageOverlay`, `VideoOverlay`, `Path`, `GeoJSON` |
| **UI & Overlays** | `references/api-ui-layers.md`, `references/panes-and-zindex.md` | `L.Marker`, `L.Popup`, `L.Tooltip`, `L.Icon`, `L.DivIcon`, Custom DOM panes |
| **Plugins Catalog** | `references/plugins-catalog.md`, `references/marker-clustering.md` | Top 13 Leaflet plugins (MarkerCluster, Geoman, AntPath, Geodesic, Heat, Minimap) |
| **Basemaps, Schemas & Services**| `references/basemaps-*`, `references/vector-tile-*` | MapTiler Planet v4 tile URLs, 512px retina offsets, schemas, and REST endpoints |

---

## 📑 Complete Reference Catalog (`references/`)

### 1. Core API Specifications & Guides
* **[api-catalog.md](api-catalog.md)** — **Exhaustive official Leaflet API directory** (55 classes, layers, UI controls, geometries) linking directly to [`leafletjs.com/reference.html`](https://leafletjs.com/reference.html).
* **[api-map.md](api-map.md)** — `L.Map` options, camera physics (`flyTo`, `fitBounds`), coordinates, and container lifecycle (`invalidateSize`).
* **[api-raster-and-vector-layers.md](api-raster-and-vector-layers.md)** — Raster tiles, WMS, image/video overlays, vector paths, GeoJSON, and SVG vs Canvas renderers.
* **[api-ui-layers.md](api-ui-layers.md)** — Markers, popups, tooltips, custom icons, radar beacons, and bundler asset resolution.
* **[api-utility-types-misc.md](api-utility-types-misc.md)** — Basic types (`LatLng`, `LatLngBounds`, `Point`), DOM utilities, browser sniffing, and transformation functions.
* **[panes-and-zindex.md](panes-and-zindex.md)** — Custom DOM pane architecture, z-index stacking hierarchy, and pointer-events isolation.
* **[examples-catalog.md](examples-catalog.md)** — API-to-Recipe directory cross-referencing all Leaflet APIs to the 28 standalone task recipes.
* **[patterns-gotchas.md](patterns-gotchas.md)** — Solutions for the top 12 Leaflet production traps (lat/lng inversion, grey tiles, 512px retina scaling, memory leaks).
* **[plugins-catalog.md](plugins-catalog.md)** — Recipes and installation for the top 13 community plugins (Geoman, MarkerCluster, AntPath, Side-by-Side).
* **[vector-tiles-and-plugins.md](vector-tiles-and-plugins.md)** — Vector tile integration via MapLibre GL Leaflet and Leaflet.VectorGrid.
* **[marker-clustering.md](marker-clustering.md)** — High-performance point density clustering with `Leaflet.markercluster`.
* **[geojson-and-markers.md](geojson-and-markers.md)** — GeoJSON data-driven styling, popups, and hover highlight patterns.
* **[events.md](events.md)** — Map lifecycle, mouse/touch interaction, layer events, and DOM propagation stopping.
* **[frameworks.md](frameworks.md)** — Integrating Leaflet in React 18, Next.js (SSR bypass), Vue 3, and Svelte.
* **[installation-and-cdn.md](installation-and-cdn.md)** — NPM package setup, bundler configuration (Vite, Webpack), CDN script tags, and CSS asset linking.
* **[prompt-benchmarks.md](prompt-benchmarks.md)** — Standardized Leaflet evaluation prompts and architectural patterns.

### 2. Basemaps, Schemas & Services
* **[vector-tile-schemas.md](vector-tile-schemas.md)** — Complete Planet v4 vector tile schema (transportation, building, water, place, poi, boundary).
* **[versions.md](versions.md)** — Core Leaflet v1.9.4, top companion plugins, CDN URLs, and V4 styles.
* **[basemaps-and-terrain.md](basemaps-and-terrain.md)** — Production endpoints for `streets-v4`, `dataviz-v4-dark`, `outdoor-v4`, `satellite-v4`, and 512px tile configs.
* **[geocoding-and-services.md](geocoding-and-services.md)** — Forward/reverse geocoding, autocomplete search, static maps, and elevation.

---

> For task-driven implementations with full HTML/CSS/JS, see **[examples/INDEX.md](../examples/INDEX.md)**.
