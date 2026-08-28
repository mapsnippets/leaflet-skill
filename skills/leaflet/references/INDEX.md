# Leaflet Skill References Index 📚🍃

This catalog lists the deep-dive architectural and API references available in the `leaflet-skill`.

---

## 📑 Complete Catalog

### Core API Specifications (Official Leaflet Reference):
1. **[api-map.md](api-map.md)** — Complete `L.Map` options, state modification (`setView`, `fitBounds`, `flyTo`, `invalidateSize`), coordinate conversions, and map properties/panes.
2. **[api-ui-layers.md](api-ui-layers.md)** — `L.Marker`, `L.Popup`, `L.Tooltip`, `L.Icon`, `L.Icon.Default`, `L.DivIcon`.
3. **[api-raster-and-vector-layers.md](api-raster-and-vector-layers.md)** — `L.TileLayer`, `L.TileLayer.WMS`, `L.ImageOverlay`, `L.VideoOverlay`, `L.SVGOverlay`, `L.Path`, `L.Polyline`, `L.Polygon`, `L.Circle`, `L.CircleMarker`, `L.Rectangle`, `L.SVG`, `L.Canvas`.
4. **[api-layers-controls-base.md](api-layers-controls-base.md)** — `L.LayerGroup`, `L.FeatureGroup`, `L.GeoJSON`, `L.GridLayer`, `L.Control` (`Zoom`, `Attribution`, `Scale`, `Layers`, `extend`), `L.Class`, `L.Evented`, `L.Layer`, `L.Handler`.
5. **[api-utility-types-misc.md](api-utility-types-misc.md)** — `L.LatLng`, `L.LatLngBounds`, `L.Point`, `L.Bounds`, `L.Util`, `L.Transformation`, `L.LineUtil`, `L.PolyUtil`, `L.DomEvent`, `L.DomUtil`, `L.Browser`, `L.CRS` (`EPSG3857`, `EPSG4326`, `Simple`), Event Objects, `noConflict`, `version`.

### Practical Guides & Modern Workflows:
6. **[vector-tiles-and-plugins.md](vector-tiles-and-plugins.md)** — Crisp vector tile styling via `@maplibre/maplibre-gl-leaflet` (`L.maplibreGL`) and `Leaflet.VectorGrid`.
7. **[geojson-and-markers.md](geojson-and-markers.md)** — Loading GeoJSON, dynamic choropleth styling, custom HTML `L.divIcon` markers, and hover highlights.
8. **[marker-clustering.md](marker-clustering.md)** — Clustering thousands of markers with `leaflet.markercluster`, custom count icons, and spiderfy behavior.
9. **[panes-and-zindex.md](panes-and-zindex.md)** — Controlling strict visual stacking using custom DOM map panes (`map.createPane`).
10. **[frameworks.md](frameworks.md)** — React (`react-leaflet`), Next.js App Router (SSR dynamic import fix), Svelte, and Vue.
11. **[patterns-gotchas.md](patterns-gotchas.md)** — Solutions for the top 10 Leaflet bugs (coordinate inversion, grey tiles, missing CSS, marker 404s).

### Basemaps, Schemas & Services:
12. **[vector-tile-schemas.md](vector-tile-schemas.md)** — Complete 9-schema vector catalog (`Planet v4`, `Outdoor`, `Contours`, `3D Buildings`, `Ocean`, `Cadastre`).
13. **[basemaps-and-terrain.md](basemaps-and-terrain.md)** — Production endpoints for `streets-v4`, `outdoor-v4`, `satellite-v4`, and 512px raster tiles.
14. **[geocoding-and-services.md](geocoding-and-services.md)** — Forward/reverse geocoding, autocomplete search, static maps, and elevation.
