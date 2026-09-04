# Leaflet Task Examples Index 🧪🗺️

> The authoritative index of **28 atomic, copy-pasteable task implementations** for Leaflet (v1.9+), curated from Leaflet documentation and community recipes and ecosystem plugins, engineered with modern MapTiler Planet v4 raster tiles.

---

## 📑 Examples by Category

### 1. 🚀 Quickstart & Core Basemaps
* **[quickstart.md](quickstart.md)** — Raster map with Streets v4, circles, polygons, and popups.
* **[layers-control.md](layers-control.md)** — Dynamic base map switcher and overlay toggles with `L.control.layers`.
* **[retina-hidpi-tiles.md](retina-hidpi-tiles.md)** — Configuring high-DPI 512px `@2x` raster tiles for crisp displays with `detectRetina: true`.
* **[zoom-levels.md](zoom-levels.md)** — Fractional zoom steps, `zoomSnap`, and scale limits.
* **[accessibility-aria.md](accessibility-aria.md)** — Accessible map navigation with keyboard controls, ARIA roles, and screen-reader titles.

### 2. 📍 Markers, Popups & Custom Styling
* **[custom-icons.md](custom-icons.md)** — Subclassing `L.Icon` with retina support, anchor points, and popup offsets.
* **[custom-radar-pulse-marker.md](custom-radar-pulse-marker.md)** — Glowing animated CSS radar beacon marker using `L.divIcon`.
* **[rotating-marker-heading.md](rotating-marker-heading.md)** — Smoothly rotating vehicle/flight marker icons based on heading or GPS bearing.
* **[marker-clustering.md](marker-clustering.md)** — Point aggregation via `L.markerClusterGroup` with spiderfy.
* **[geojson-choropleth.md](geojson-choropleth.md)** — Dynamic choropleth polygon fill thresholds, hover highlight, and interactive legend.

### 3. 📱 Mobile, Custom Panes & Overlays
* **[mobile-geolocation.md](mobile-geolocation.md)** — Mobile fullscreen viewport and GPS location tracking.
* **[map-panes.md](map-panes.md)** — Custom panes sandwich architecture (road labels over polygons).
* **[image-overlay-floorplan.md](image-overlay-floorplan.md)** — Anchoring architectural floorplans with `L.imageOverlay`.
* **[side-by-side.md](side-by-side.md)** — Split-screen layer swipe comparison slider between satellite and street tiles.
* **[fullscreen-toggle.md](fullscreen-toggle.md)** — Fullscreen map toggle control.

### 4. 🌐 Enterprise Services & Vector Tiles
* **[wms-layers.md](wms-layers.md)** — OGC `L.tileLayer.wms` with transparent overlays.
* **[wms-getfeatureinfo-click.md](wms-getfeatureinfo-click.md)** — Querying attribute data via OGC WMS `GetFeatureInfo` on click.
* **[geotiff-raster-layer.md](geotiff-raster-layer.md)** — Rendering Cloud-Optimized GeoTIFF raster in Leaflet with `georaster-layer-for-leaflet`.
* **[vector-grid-mvt.md](vector-grid-mvt.md)** — Native Canvas rendering of binary Mapbox Vector Tiles (.pbf) with `Leaflet.VectorGrid`.
* **[crs-simple.md](crs-simple.md)** — Non-geographical flat Cartesian coordinate maps (`L.CRS.Simple`).

### 5. ✍️ Spatial Digitization, Heatmaps & Routing
* **[geoman-geometry-editing.md](geoman-geometry-editing.md)** — Complete vector digitizing suite (drawing, editing vertices, cutting polygons).
* **[gpx-track-viewer.md](gpx-track-viewer.md)** — Parsing and visualizing GPS tracks and waypoints using `leaflet-gpx`.
* **[interactive-measure-tool.md](interactive-measure-tool.md)** — Interactive geodesic distance and area measurement control (`leaflet-measure`).
* **[geodesic-measure.md](geodesic-measure.md)** — Spherical great-circle navigation lines and accurate geodesic distances.
* **[heatmaps.md](heatmaps.md)** — Continuous point density heatmaps via WebGL / `simpleheat`.
* **[animated-polyline-ant-path.md](animated-polyline-ant-path.md)** — Animated marching ants polyline route tracking.
* **[minimap-overview.md](minimap-overview.md)** — Synchronized corner mini-map context control.
* **[custom-control.md](custom-control.md)** — Subclassing `L.Control.extend` with `disableClickPropagation`.
