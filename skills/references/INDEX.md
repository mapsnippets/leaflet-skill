# Leaflet Skill References Index 📚🍃

> Curated index of architectural specifications, core API references, and official step-by-step example tutorials in `leaflet-skill`. Load on demand.

Maintained by **[MapSnippets](https://mapsnippets.org/)** — Open-source geospatial snippets, guides, and agent tools.

---

## 🔎 Fast Search & Prefix Conventions

Target your `grep` or glob searches in `references/` using these prefixes:

| Category | File Prefix | Contents |
| :--- | :--- | :--- |
| **Core API & Architecture** | `api-*` | Authoritative specifications for Map, UI Layers, Vector Layers, Controls, and Utilities |
| **Official Examples & Recipes** | `examples-leaflet-*` | Atomic, self-contained, copy-pasteable tutorials with full HTML, CSS, and native JavaScript |
| **Basemaps, Schemas & Services**| `basemaps-*`, `vector-tile-*`, `geocoding-*` | MapTiler Planet v4 tile URLs, vector schemas, and REST endpoints |

---

## 📑 Complete Catalog

### 1. Official Step-by-Step Examples (`examples-leaflet-*`):
1. **[examples-leaflet-quickstart.md](examples-leaflet-quickstart.md)** — Map setup, high-DPI raster tiles, markers, circles, polygons, popups, and click events.
2. **[examples-leaflet-custom-icons.md](examples-leaflet-custom-icons.md)** — Defining custom `L.Icon` classes, retina `@2x`, anchor offsets, and shadow alignment.
3. **[examples-leaflet-geojson-choropleth.md](examples-leaflet-geojson-choropleth.md)** — US population density, dynamic color scale, hover highlight, custom info HUD, and legend.
4. **[examples-leaflet-layers-control.md](examples-leaflet-layers-control.md)** — `L.control.layers`, mutually exclusive base maps, and toggleable overlay layer groups.
5. **[examples-leaflet-mobile-geolocation.md](examples-leaflet-mobile-geolocation.md)** — Mobile fullscreen viewport, user location tracking via `map.locate`, and accuracy circle.
6. **[examples-leaflet-map-panes.md](examples-leaflet-map-panes.md)** — Custom DOM panes (`map.createPane`), sandwich architecture (labels over vector polygons).
7. **[examples-leaflet-wms-layers.md](examples-leaflet-wms-layers.md)** — Connecting to OGC Web Map Services via `L.tileLayer.wms` with transparent overlays.
8. **[examples-leaflet-crs-simple.md](examples-leaflet-crs-simple.md)** — `L.CRS.Simple`, pixel coordinates for indoor floorplans, gaming maps, and non-geographic imagery.
9. **[examples-leaflet-custom-control.md](examples-leaflet-custom-control.md)** — Creating custom HUD buttons via `L.Control.extend` with click propagation prevention.
10. **[examples-leaflet-marker-clustering.md](examples-leaflet-marker-clustering.md)** — `leaflet.markercluster`, 10k+ chunked loading, custom count icons, and spiderfy.
11. **[examples-leaflet-heatmaps.md](examples-leaflet-heatmaps.md)** — `leaflet.heat` HTML5 Canvas heatmap surfaces, intensity gradients, radius, and blur.
12. **[examples-leaflet-side-by-side.md](examples-leaflet-side-by-side.md)** — `leaflet-side-by-side` swipe slider split comparison between satellite and street tiles.

### 2. Core API Specifications (Official Leaflet Reference):
13. **[api-map.md](api-map.md)** — Complete `L.Map` options, state modification (`setView`, `fitBounds`, `flyTo`, `invalidateSize`), coordinate conversions, and map properties/panes.
14. **[api-ui-layers.md](api-ui-layers.md)** — `L.Marker`, `L.Popup`, `L.Tooltip`, `L.Icon`, `L.Icon.Default`, `L.DivIcon`.
15. **[api-raster-and-vector-layers.md](api-raster-and-vector-layers.md)** — `L.TileLayer`, `L.TileLayer.WMS`, `L.ImageOverlay`, `L.VideoOverlay`, `L.SVGOverlay`, `L.Path`, `L.Polyline`, `L.Polygon`, `L.Circle`, `L.CircleMarker`, `L.Rectangle`, `L.SVG`, `L.Canvas`.
16. **[api-layers-controls-base.md](api-layers-controls-base.md)** — `L.LayerGroup`, `L.FeatureGroup`, `L.GeoJSON`, `L.GridLayer`, `L.Control` (`Zoom`, `Attribution`, `Scale`, `Layers`, `extend`), `L.Class`, `L.Evented`, `L.Layer`, `L.Handler`.
17. **[api-utility-types-misc.md](api-utility-types-misc.md)** — `L.LatLng`, `L.LatLngBounds`, `L.Point`, `L.Bounds`, `L.Util`, `L.Transformation`, `L.LineUtil`, `L.PolyUtil`, `L.DomEvent`, `L.DomUtil`, `L.Browser`, `L.CRS` (`EPSG3857`, `EPSG4326`, `Simple`), Event Objects, `noConflict`, `version`.

### 3. Practical Guides & Ecosystem Workflows:
18. **[vector-tiles-and-plugins.md](vector-tiles-and-plugins.md)** — Crisp vector tile styling via `@maplibre/maplibre-gl-leaflet` (`L.maplibreGL`) and `Leaflet.VectorGrid`.
19. **[geojson-and-markers.md](geojson-and-markers.md)** — Loading GeoJSON, dynamic choropleth styling, custom HTML `L.divIcon` markers, and hover highlights.
20. **[marker-clustering.md](marker-clustering.md)** — Clustering thousands of markers with `leaflet.markercluster`, custom count icons, and spiderfy behavior.
21. **[panes-and-zindex.md](panes-and-zindex.md)** — Controlling strict visual stacking using custom DOM map panes (`map.createPane`).
22. **[frameworks.md](frameworks.md)** — React (`react-leaflet`), Next.js App Router (SSR dynamic import fix), Svelte, and Vue.
23. **[plugins-catalog.md](plugins-catalog.md)** — Working recipes for the top 10 plugins (heatmaps, geoman drawing, split comparison, minimap, omnivore, routing).
24. **[patterns-gotchas.md](patterns-gotchas.md)** — Solutions for the top 10 Leaflet bugs (coordinate inversion, grey tiles, missing CSS, marker 404s).
25. **[events.md](events.md)** — Comprehensive DOM and map event listeners (`click`, `moveend`, `zoomlevelschange`, `layeradd`).
26. **[prompt-benchmarks.md](prompt-benchmarks.md)** — Standardized Leaflet evaluation prompts and verified patterns.

### 4. Basemaps, Schemas & Services:
27. **[vector-tile-schemas.md](vector-tile-schemas.md)** — Complete 9-schema vector catalog (`Planet v4`, `Outdoor`, `Contours`, `3D Buildings`, `Ocean`, `Cadastre`).
28. **[basemaps-and-terrain.md](basemaps-and-terrain.md)** — Production endpoints for `streets-v4`, `outdoor-v4`, `satellite-v4`, and 512px raster tiles.
29. **[geocoding-and-services.md](geocoding-and-services.md)** — Forward/reverse geocoding, autocomplete search, static maps, and elevation.
