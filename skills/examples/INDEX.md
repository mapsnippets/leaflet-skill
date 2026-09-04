# Leaflet Official Task Examples Index 🧪🗺️

> The authoritative index of **22 atomic, copy-pasteable task implementations** for Leaflet (v1.9+), extracted directly from official Leaflet tutorials and top ecosystem plugins, engineered with modern MapTiler Planet v4 raster and vector tiles.

---

## 📑 Examples by Category

### 1. 🗺️ Quickstart & Core Basemaps
* **[quickstart.md](quickstart.md)** — Basic raster map with MapTiler Streets v4, circle overlays, polygons, and popups.
* **[layers-control.md](layers-control.md)** — Dynamic base map switcher and overlay toggles with `L.control.layers`.
* **[zoom-levels.md](zoom-levels.md)** — Fine-tuning fractional zoom steps, `zoomSnap`, and scale limits.
* **[accessibility-aria.md](accessibility-aria.md)** — Accessible map navigation with keyboard controls, ARIA roles, and screen-reader titles.

### 2. 📍 Markers, Popups & Custom Styling
* **[custom-icons.md](custom-icons.md)** — Subclassing `L.Icon` with retina support, anchor points, and popup offsets.
* **[custom-radar-pulse-marker.md](custom-radar-pulse-marker.md)** — Glowing animated CSS radar beacon marker using `L.divIcon`.
* **[marker-clustering.md](marker-clustering.md)** — High-performance point aggregation via `L.markerClusterGroup` with spiderfy.
* **[geojson-choropleth.md](geojson-choropleth.md)** — Dynamic choropleth polygon fill thresholds, hover highlight, and interactive legend.

### 3. 📱 Mobile, Custom Panes & Overlays
* **[mobile-geolocation.md](mobile-geolocation.md)** — Device GPS geolocation with `map.locate` and location accuracy circles.
* **[map-panes.md](map-panes.md)** — Custom z-index panes ensuring road labels render cleanly above vector polygons.
* **[image-overlay-floorplan.md](image-overlay-floorplan.md)** — Anchoring architectural floorplans or aerial photos with `L.imageOverlay`.
* **[side-by-side.md](side-by-side.md)** — Split-screen layer swipe comparison slider between Satellite v4 and Streets v4.
* **[fullscreen-toggle.md](fullscreen-toggle.md)** — Fullscreen map toggle control conforming to standard Fullscreen API.

### 4. 🌐 Enterprise Services & Vector Tiles
* **[wms-layers.md](wms-layers.md)** — Integrating enterprise OGC `L.tileLayer.wms` over MapTiler basemaps.
* **[vector-grid-mvt.md](vector-grid-mvt.md)** — Native Canvas rendering of binary Mapbox Vector Tiles (.pbf) with `Leaflet.VectorGrid`.
* **[crs-simple.md](crs-simple.md)** — Non-geographical flat Cartesian coordinate maps (`L.CRS.Simple`) for high-res images and games.

### 5. ✍️ Spatial Digitization, Heatmaps & Routing
* **[geoman-geometry-editing.md](geoman-geometry-editing.md)** — Complete vector digitizing suite (drawing, editing vertices, cutting polygons).
* **[geodesic-measure.md](geodesic-measure.md)** — Spherical great-circle navigation lines and accurate geodesic distances.
* **[heatmaps.md](heatmaps.md)** — Continuous point density heatmaps via WebGL / `simpleheat`.
* **[animated-polyline-ant-path.md](animated-polyline-ant-path.md)** — Animated marching ants polyline route tracking.
* **[minimap-overview.md](minimap-overview.md)** — Synchronized corner mini-map context control.
* **[custom-control.md](custom-control.md)** — Subclassing `L.Control.extend` with `disableClickPropagation` for custom UI buttons.
