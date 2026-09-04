# Leaflet Skill References Index 📚🍃

> Curated index of architectural specifications, core API references, and official step-by-step example tutorials in `leaflet-skill`. Load on demand.

Maintained by **[MapSnippets](https://mapsnippets.org/)** — Open-source geospatial snippets, guides, and agent tools.

---

## 🔎 Fast Search & Topic Routing

| Topic Area | Directory / Prefix | Contents |
| :--- | :--- | :--- |
| **Official Task Examples** | **[examples/INDEX.md](../examples/INDEX.md)** | **22 atomic official examples** with full HTML, CSS, and native `L.*` JS across Quickstart, Panes, Mobile, GeoJSON, Clustering, Geoman, and Overlays |
| **Core API & Architecture** | `references/api-*` | Authoritative specifications for Map, UI Layers, Vector Layers, Controls, and Utilities |
| **Ecosystem & Workflows** | `references/geojson-*`, `references/panes-*`, `references/marker-*` | Deep guides for clustering, custom panes, GeoJSON styling, and frameworks |
| **Basemaps, Schemas & Services**| `references/basemaps-*`, `references/vector-tile-*` | MapTiler Planet v4 tile URLs, vector schemas, and REST endpoints |

---

## 📑 Complete Reference Catalog (`references/`)

### 1. Core API Specifications (Official Leaflet Reference)
* **[api-map.md](api-map.md)** — Complete `L.Map` options, state modification (`setView`, `fitBounds`, `flyTo`, `invalidateSize`), coordinate conversions, and map properties/panes.
* **[api-ui-layers.md](api-ui-layers.md)** — `L.Marker`, `L.Popup`, `L.Tooltip`, `L.Icon`, `L.Icon.Default`, `L.DivIcon`.
* **[api-raster-and-vector-layers.md](api-raster-and-vector-layers.md)** — `L.TileLayer`, `L.TileLayer.WMS`, `L.ImageOverlay`, `L.VideoOverlay`, `L.SVGOverlay`, `L.Path`, `L.Polyline`, `L.Polygon`, `L.Circle`, `L.CircleMarker`, `L.Rectangle`, `L.SVG`, `L.Canvas`.
* **[api-layers-controls-base.md](api-layers-controls-base.md)** — `L.LayerGroup`, `L.FeatureGroup`, `L.GeoJSON`, `L.GridLayer`, `L.Control` (`Zoom`, `Attribution`, `Scale`, `Layers`, `extend`), `L.Class`, `L.Evented`, `L.Layer`, `L.Handler`.
* **[api-utility-types-misc.md](api-utility-types-misc.md)** — `L.LatLng`, `L.LatLngBounds`, `L.Point`, `L.Bounds`, `L.Util`, `L.Transformation`, `L.LineUtil`, `L.PolyUtil`, `L.DomEvent`, `L.DomUtil`, `L.Browser`, `L.CRS` (`EPSG3857`, `EPSG4326`, `Simple`), Event Objects, `noConflict`, `version`.

### 2. Practical Guides & Ecosystem Workflows
* **[vector-tiles-and-plugins.md](vector-tiles-and-plugins.md)** — Crisp vector tile styling via `@maplibre/maplibre-gl-leaflet` (`L.maplibreGL`) and `Leaflet.VectorGrid`.
* **[geojson-and-markers.md](geojson-and-markers.md)** — Loading GeoJSON, dynamic choropleth styling, custom HTML `L.divIcon` markers, and hover highlights.
* **[marker-clustering.md](marker-clustering.md)** — Clustering thousands of markers with `leaflet.markercluster`, custom count icons, and spiderfy behavior.
* **[panes-and-zindex.md](panes-and-zindex.md)** — Controlling strict visual stacking using custom DOM map panes (`map.createPane`).
* **[frameworks.md](frameworks.md)** — React (`react-leaflet`), Next.js App Router (SSR dynamic import fix), Svelte, and Vue.
* **[plugins-catalog.md](plugins-catalog.md)** — Working recipes for the top 10 plugins (heatmaps, geoman drawing, split comparison, minimap, omnivore, routing).
* **[patterns-gotchas.md](patterns-gotchas.md)** — Solutions for the top 10 Leaflet bugs (coordinate inversion, grey tiles, missing CSS, marker 404s).
* **[events.md](events.md)** — Comprehensive DOM and map event listeners (`click`, `moveend`, `zoomlevelschange`, `layeradd`).
* **[prompt-benchmarks.md](prompt-benchmarks.md)** — Standardized Leaflet evaluation prompts and verified patterns.

### 3. Basemaps, Schemas & Services
* **[vector-tile-schemas.md](vector-tile-schemas.md)** — Complete 9-schema vector catalog (`Planet v4`, `Outdoor`, `Contours`, `3D Buildings`, `Ocean`, `Cadastre`).
* **[basemaps-and-terrain.md](basemaps-and-terrain.md)** — Production endpoints for `streets-v4`, `outdoor-v4`, `satellite-v4`, and 512px raster tiles.
* **[geocoding-and-services.md](geocoding-and-services.md)** — Forward/reverse geocoding, autocomplete search, static maps, and elevation.

---

> For task-driven implementations with full HTML/CSS/JS, see **[examples/INDEX.md](../examples/INDEX.md)**.
