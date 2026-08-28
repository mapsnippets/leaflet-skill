# Leaflet Skill References Index 📚🍃

This catalog lists the deep-dive architectural and API references available in the `leaflet-skill`.

---

## 📑 Complete Catalog

### Core API Specifications (Official Leaflet Reference):
1. **[api-map-and-views.md](api-map-and-views.md)** — Complete `L.Map` options, state modification (`setView`, `fitBounds`, `flyTo`), coordinate conversions (`latLngToContainerPoint`), and map event dictionary.
2. **[api-ui-and-vector-layers.md](api-ui-and-vector-layers.md)** — `L.Marker`, `L.Popup`, `L.Tooltip`, `L.Icon`, `L.DivIcon`, `L.Path`, `L.Polyline`, `L.Polygon`, `L.Circle`, `L.CircleMarker`, `L.Rectangle`.
3. **[api-layers-and-controls.md](api-layers-and-controls.md)** — `L.TileLayer`, `L.TileLayer.WMS`, `L.ImageOverlay`, `L.VideoOverlay`, `L.LayerGroup`, `L.FeatureGroup`, `L.GeoJSON`, and UI controls.
4. **[api-types-crs-utilities.md](api-types-crs-utilities.md)** — `L.LatLng`, `L.LatLngBounds`, `L.Point`, `L.Bounds`, `L.CRS` (`EPSG3857`, `EPSG4326`, `Simple`), `L.DomUtil`, and `L.DomEvent`.

### Practical Guides & Modern Workflows:
5. **[vector-tiles-and-plugins.md](vector-tiles-and-plugins.md)** — Crisp vector tile styling via `@maplibre/maplibre-gl-leaflet` (`L.maplibreGL`) and `Leaflet.VectorGrid`.
6. **[geojson-and-markers.md](geojson-and-markers.md)** — Loading GeoJSON, dynamic choropleth styling, custom HTML `L.divIcon` markers, and hover highlights.
7. **[marker-clustering.md](marker-clustering.md)** — Clustering thousands of markers with `leaflet.markercluster`, custom count icons, and spiderfy behavior.
8. **[panes-and-zindex.md](panes-and-zindex.md)** — Controlling strict visual stacking using custom DOM map panes (`map.createPane`).
9. **[frameworks.md](frameworks.md)** — React (`react-leaflet`), Next.js App Router (SSR dynamic import fix), Svelte, and Vue.
10. **[patterns-gotchas.md](patterns-gotchas.md)** — Solutions for the top 10 Leaflet bugs (coordinate inversion, grey tiles, missing CSS, marker 404s).

### Basemaps, Schemas & Services:
11. **[vector-tile-schemas.md](vector-tile-schemas.md)** — Complete 9-schema vector catalog (`Planet v4`, `Outdoor`, `Contours`, `3D Buildings`, `Ocean`, `Cadastre`).
12. **[basemaps-and-terrain.md](basemaps-and-terrain.md)** — Production endpoints for `streets-v4`, `outdoor-v4`, `satellite-v4`, and 512px raster tiles.
13. **[geocoding-and-services.md](geocoding-and-services.md)** — Forward/reverse geocoding, autocomplete search, static maps, and elevation.
